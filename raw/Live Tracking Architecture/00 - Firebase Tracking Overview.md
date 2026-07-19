---
title: Live Tracking — Firebase Architecture Overview
tags: [tracking, firebase, drayos-35867, architecture]
ticket: DRAYOS-35867
created: 2026-07-19
status: verified (code + data)
---

# Live Tracking — Firebase Architecture Overview

Context: DRAYOS-35867 (FD #39955) — Whisk O/O drivers show status but **no live GPS marker**. Investigation mapped the whole realtime-tracking architecture across viewer→driver flows.

## Two independent pipes

Every tracking screen is fed by two separate pipes:

| Pipe | What | Path |
|------|------|------|
| **A — realtime marker** | moving truck icon | driver app **writes Firebase RTDB** directly (SDK) → FE **subscribes** via `onValue` |
| **B — trail / history** | breadcrumb polyline | app → tracking-api `POST /mobile/history` → Mongo `driver_tracking_histories` → FE reads back over backend REST |

The DRAYOS-35867 fix repaired **Pipe B** (trail + `currentDriverLocation`) — verified live. The open item is **Pipe A** (realtime marker) for O/O.

## The Firebase DBs (env-verified via devops parameter-viewer)

| Env key | URL | Purpose |
|---------|-----|---------|
| `FIREBASE_DATABASEURL` | `portpro-294915` | **main** (broker's own DB) |
| `MOBILE_FIREBASE_DATABASEURL` | `driver-location-portpro` | **mobile** — where every driver app writes GPS |
| `ELD_FIREBASE_DATABASEURL` | `portpro-eld` | ELD |
| `EU_FIREBASE_DATABASEURL` | `portpro-eu` | EU |

Legacy/dead: `axle-178513` (old default), `portpro-shipos-dev` (dead — REFUTED as the bug).

**Key fact:** `mobileFirebase` (`driver-location-portpro`) is **ONE shared/global DB** for all carriers. Not per-carrier, not per-driver. Node path is carrier-scoped:

```
{carrierUSERid}/currentLocation/{driverUSERid}
```

Both are USER `_id`s (not driver RECORD id). `currentDriverLocation` scalar lives on the driver RECORD (Pipe B).

## App-origin fingerprint: `bearing`

- **In-house app** (`PortPro-remastered-flutter`) → writes `bearing` in payload
- **O/O app** (`owner-operator-mobile`) → does NOT write `bearing`

Used to tell which app authored a Firebase node.

## Building blocks (shared by all flows)

**A. FE instances** — `portpro-frontends/src/config/index.js`
- `:15` `main` = `REACT_APP_FIREBASE_DATABASEURL` → portpro-294915
- `:71-74` `mobileFirebase` = `REACT_APP_MOBILE_FIREBASE_DATABASEURL` → driver-location-portpro
- `:53-61` `getNewFirebaseInstanceByConfig(config)` → app pointed at any handed config

**B. Instance picker** — `src/hooks/firebase/useFirebaseRef.js:16-39` (precedence)
```
:25  if (firebaseConfig) → getNewFirebaseInstanceByConfig(...,"portpro-brokerage")  // WINS
:30  if (isELD)          → eldFirebase
:34  if (isMobile)       → mobileFirebase
:38  else                → main
```

**C. Namespace + subscription** — `src/pages/trucker/Tracking/Components/LiveMarkers/EachLiveDriverWithoutELD.js:207-227`
```js
:208  namespace = `${currentCarrierId}/currentLocation/${driver._id}`
:210  carrier = brokerCarrierDriverIdMapper[driver._id]
:211  if (_isBrokerContainerTrackingEnabled && carrier)
:212      namespace = `${carrier}/currentLocation/${driver._id}`
:215  ref = getFirebaseRefByNameSpace({ overrideNamespace, firebaseConfig, isMobile:true })
:222  ref.on("value", handleDriverLocationUpdate)
```
`isMobile` always true here → without `firebaseConfig`, lands on `mobileFirebase`; with one, `firebaseConfig` overrides.

**D. Inputs**
- `brokerCarrierDriverIdMapper` ← `load-firebase-instances` API resp (`LoadList.js:131-132`)
- `firebaseConfig` ← **only** `fetchDrayosFirebaseConfig` (`useContainersTrackingSidePanel.js:159-164`) → `getDrayosFirebaseConfig` → **MAIN config** (BE `getFirebaseConfig:828`). **Load-info tab never sets it** → stays `null` (`driverReducer.js:70`).

## Backend resolver — `portpro-backend tms-controller.js`

`getLoadFirebaseInstances` (~`:11824`) splits tenders (DRAYOS-35867):
```js
:11862  read broker's own Setting.isBrokerContainerTrackingEnabled
:11875  isDrayosDestination = drayosCarrier.connectDestination === DRAYOS
:11876  if (flag && isDrayosDestination) localMapper[key]  // NEW same-deployment local resolve
:11878  else                             connectMapper[key] // connected carrier → Connect proxy
:11886  connectMapper → CPAHookCall(isPortproConnect) → vendor instance
:11904  localMapper   → CustomerApiController.resolveDrayosFirebaseInstances (no Connect hop)
```
`resolveDrayosFirebaseInstances` (`customer-api-controller.js:483-552`) returns `{driver, carrierId}` in THIS deployment's id-space.

`getFirebaseConfig` (`:824-835`) returns `FIREBASE_DATABASEURL` = **MAIN**. `getDrayosFirebaseConfig` (`:11915`) proxies to vendor `get-drayos-firebase-config` via CPA.

## The 3 viewer→driver flows

| Flow | namespace source | firebaseConfig | Instance read | GPS write DB | Match? |
|------|-----------------|----------------|---------------|--------------|--------|
| 1 Carrier→own driver | own carrier | null | mobileFirebase | driver-location-portpro | ✅ |
| 2 Broker→connected carrier | vendor (Connect) | vendor cfg | vendor Firebase | vendor DB | ✅ |
| 3a Hybrid→OO (load-info tab) | OO (local resolve) | null | mobileFirebase | driver-location-portpro | ✅ |
| 3b Hybrid→OO (containers page) | OO (local resolve) | **MAIN** | portpro-294915 | driver-location-portpro | ❌ |

## The one real code bug — 3b (containers page, O/O)

Containers page forces `firebaseConfig` = `getDrayosFirebaseConfig` = **MAIN** (portpro-294915). `picker:25` lets `firebaseConfig` override `isMobile` → reads MAIN instead of the mobile GPS DB (`driver-location-portpro`) → **no marker**.

Load-info tab (3a) dodges it by never setting `firebaseConfig` → picker falls to `isMobile` → mobileFirebase → correct.

**Candidate fixes:**
1. `getDrayosFirebaseConfig` returns the **mobile** config, OR
2. In `picker` merge `isMobile` DB URL when `firebaseConfig` lacks a mobile databaseURL.

## Verified / refuted

- ✅ FE reads `driver-location-portpro` as MOBILE (bundle grep `main.535582f0.js` 14×, param store)
- ✅ Pipe B fix live-verified (currentDriverLocation `[85.302,27.673]`, trail 539→779; E2E 11/11)
- ❌ "shipos-dev DB mismatch" — REFUTED (direct RTDB read null)
- ❌ "FE reads wrong DB / MOBILE env unset" — REFUTED (baked in bundle)
- ⏳ **Open:** where O/O app writes Firebase live location — blocked (emulator produced no fresh write because **pre-prod backend was down**). Needs a live O/O write with backend up.

## Related
- [[01 - Flow 1 - Carrier to Own Driver]]
- [[02 - Flow 2 - Broker to Connected Carrier]]
