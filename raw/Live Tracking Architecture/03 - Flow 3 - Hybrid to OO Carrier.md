---
title: Flow 3 — Hybrid → OO Carrier (DRAYOS-35867 core)
tags: [tracking, firebase, flow, owner-operator, drayos-35867]
ticket: DRAYOS-35867
created: 2026-07-19
status: verified (code + data)
---

# Flow 3 — Hybrid → Owner-Operator Carrier

**This is the DRAYOS-35867 bug flow.** A **hybrid** account (broker + carrier) tenders to a **same-deployment O/O carrier** (`connectDestination === DRAYOS`). The O/O driver runs `owner-operator-mobile`.

Two surfaces diverge:
- **3a — Load-info Tracking tab** → works ✅
- **3b — Containers page** → no marker ❌

Overview: [[00 - Firebase Tracking Overview]]. Contrast [[02 - Flow 2 - Broker to Connected Carrier|Flow 2]] (cross-instance, HTTP hop to the Drayos BE `customer-api` module).

---

## Backend resolution (no diagram)

Same `portpro-backend` deployment plays both roles (`tms` module = Broker/Hybrid BE, `customer-api` module = Drayos BE):

- **Driver-resolve** — `tms-controller.js:11824 getLoadFirebaseInstances`: flag ON + `connectDestination==DRAYOS` → `localMapper` → **in-process call** to `customer-api-controller.js:483 resolveDrayosFirebaseInstances()`. **No HTTP** (unlike Flow 2's `CPAHookCall`).
- **Config (containers only)** — `tms-controller.js:11915 getDrayosFirebaseConfig` → `CPAHookCall(isPortproConnect)` → HTTP `connectUrl || PORTPRO_CONNECT_URL` `v1/broker/get-drayos-firebase-config` → loops back to the same backend's `customer-api-controller.js:588` → `getFirebaseConfig()` = **MAIN**. This HTTP round-trip is what breaks Page 1.

---

## WRITE half — O/O app (`owner-operator-mobile`)

### Pipe A — Firebase (no bearing)
`lib/services/firebase_service.dart:95`
```dart
final _locationData = {
  "location": [locationData.lng, locationData.lat],
  "last": <ISO utc>, "status": …, "ref": …, "type": …, "prevType": …,
  // ❗ NO "bearing" key — O/O fingerprint
};
DatabaseReference _ref = _database.ref().child('$userID/currentLocation/$defaultDriverId');
await _ref.set(_updatedData);
```
- `userID` = O/O **carrier** USER id (O/O logs in as carrier)
- `defaultDriverId` = default driver USER id
- `_database` = `appServer.databaseUrl` = **`driver-location-portpro`** (after `updateFirebaseDatabase()`)

Node = `{ooCarrierUser}/currentLocation/{defaultDriverUser}` in the **mobile** DB. Same shape as Flow 1 — just no `bearing`.

### Pipe B — trail (the DRAYOS-35867 fix)

`owner-operator-mobile` → `POST /mobile/history?ownerOperator=true` → `portpro-tracking-api routes/index.js:58` → `mobileAuthRouter (mobileAuth.js:76)` → `ownerOperator==true` → `ooAuth (ooAuth.js)` → `trackingHistoryController` → inserts history **and** updates `currentDriverLocation.coordinates` on the driver RECORD (both in Mongo).

`ooAuth` (`middleware/ooAuth.js`) resolves:
- carrier user (`role:'carrier'`) → `req.body.carrier`
- `ownerOperatorConfig.defaultDriver` (USER id) → `req.body.driver`
- driver RECORD id → `req.ownerOperatorDriverRecordId`
- sets `req.isOwnerOperator = true`

---

## READ half — two pages, two outcomes

Both pages call the same `EachLiveDriverWithoutELD.js:207` and resolve the **same namespace** `{ooCarrier}/currentLocation/{ooDriver}`. They differ only in **which `firebaseConfig` reaches the picker** — and that decides which Firebase DB is read.

```js
// EachLiveDriverWithoutELD.js:207 — identical on both pages
const carrier = brokerCarrierDriverIdMapper[driver._id];   // ooCarrier (local resolve)
if (_isBrokerContainerTrackingEnabled && carrier)
  namespace = `${ooCarrier}/currentLocation/${ooDriver}`;
const ref = getFirebaseRefByNameSpace({ overrideNamespace: namespace, firebaseConfig, isMobile: true });
```

---

### 📄 Page 1 — Tracking › Containers  (`/tracking/containers`)  →  ❌ no marker (3b)

This page **fetches** a `firebaseConfig` (= MAIN) and passes it down. Picker `:25` lets it win → reads the wrong DB.

```mermaid
flowchart TD
  P["/tracking/containers<br/>TrackingContainersPage.js:531<br/>&lt;LiveContainerMarkersList /&gt;"]
  SP["useContainersTrackingSidePanel.js"]
  MAP["...:453  brokerCarrierDriverIdMapper[ooDriver] = ooCarrier"]
  CFG["...:159  fetchDrayosFirebaseConfig()<br/>→ getDrayosFirebaseConfig = MAIN"]
  LCM["LiveContainerMarkersList.js:184<br/>firebaseConfig = stateDriver.firebaseConfig (= MAIN)"]
  ELD["EachLiveDriverWithoutELD.js:212<br/>namespace {ooCarrier}/currentLocation/{ooDriver}"]
  PICK["useFirebaseRef.js:25<br/>firebaseConfig SET → getNewFirebaseInstanceByConfig(MAIN)"]
  FBx[("🔥 portpro-294915 (MAIN)<br/>empty for this node")]
  X(("❌ no marker"))

  P --> SP
  SP --> MAP --> ELD
  SP --> CFG --> LCM --> ELD
  ELD --> PICK --> FBx --> X
```

---

### 📄 Page 2 — Load info Modal › Tracking tab  (`LoadTrackingHistory`)  →  ✅ marker (3a)

This page **never fetches** a config → `firebaseConfig` stays `null` → picker falls to `isMobile` → correct DB.

```mermaid
flowchart TD
  P["Load info Modal → Tracking tab<br/>LoadTrackingHistory/index.js:60<br/>&lt;LiveTrucksMarkers /&gt;"]
  CFG["index.js:32<br/>firebaseConfig = stateDriver?.firebaseConfig ?? null<br/>(no fetchDrayosFirebaseConfig here → NULL)"]
  LL["LoadList.js:132<br/>brokerCarrierDriverIdMapper[ooDriver] = ooCarrier"]
  ELD["EachLiveDriverWithoutELD.js:212<br/>namespace {ooCarrier}/currentLocation/{ooDriver}"]
  PICK["useFirebaseRef.js:34<br/>firebaseConfig null → isMobile → mobileFirebase"]
  FBm[("🔥 driver-location-portpro (MOBILE)<br/>where O/O writes")]
  OK(("✅ marker shows"))

  P --> CFG --> ELD
  LL --> ELD
  ELD --> PICK --> FBm --> OK
```

---

### Side-by-side

| | 📄 Page 1 · Containers | 📄 Page 2 · Load info modal |
|--|------------------------|-----------------------------|
| entry | `TrackingContainersPage.js:531` | `LoadTrackingHistory/index.js:60` |
| markers list | `LiveContainerMarkersList` | `LiveTrucksMarkersList` |
| mapper source | `useContainersTrackingSidePanel:453` | `LoadList.js:132` |
| `firebaseConfig` | `getDrayosFirebaseConfig` = **MAIN** | `null` (never fetched) |
| picker branch | `:25` firebaseConfig wins | `:34` isMobile |
| reads DB | `portpro-294915` (MAIN) | `driver-location-portpro` (MOBILE) |
| O/O GPS is in | MOBILE | MOBILE |
| **result** | ❌ **no marker** | ✅ **marker shows** |

---

## Root cause + fix

O/O GPS lives in **`driver-location-portpro`** (mobile). The containers page reads **MAIN** because `getDrayosFirebaseConfig` returns `getFirebaseConfig()` (= `FIREBASE_DATABASEURL`), and picker `:25` lets `firebaseConfig` override `isMobile`.

**Candidate fixes:**
1. `getDrayosFirebaseConfig` returns the **mobile** config (`MOBILE_FIREBASE_DATABASEURL`), OR
2. picker (`useFirebaseRef.js:25`) merges the mobile `databaseURL` when `firebaseConfig` lacks one and `isMobile` is set.

> Same root bug as [[02 - Flow 2 - Broker to Connected Carrier|Flow 2]] containers page — `getDrayosFirebaseConfig` = MAIN, not mobile.

---

## Verified vs open
- ✅ O/O app node = `{ooCarrier}/currentLocation/{ooDriver}`, **no bearing** (`firebase_service.dart:95-112`)
- ✅ local resolve path (no Connect hop) — `getLoadFirebaseInstances:11876` + `resolveDrayosFirebaseInstances:483`
- ✅ `ooAuth` + trail fix live-verified (currentDriverLocation `[85.302,27.673]`, trail 539→779; E2E 11/11)
- ✅ 3a load-info tab reads mobileFirebase (correct)
- ⏳ OPEN: live O/O Firebase write not observed — emulator produced none because **pre-prod backend was down**. Needs a live write with backend up to close 100%.

## Related
- [[00 - Firebase Tracking Overview]]
- [[01 - Flow 1 - Carrier to Own Driver]]
- [[02 - Flow 2 - Broker to Connected Carrier]]
