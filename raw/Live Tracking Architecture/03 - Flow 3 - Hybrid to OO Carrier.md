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

## Repo hop chain

Unlike Flow 2, driver resolution is an **in-process function call** — same deployment, **no HTTP hop** (`resolveDrayosFirebaseInstances` is called directly, not via `CPAHookCall`).

```mermaid
flowchart LR
  FE["🖥️ Hybrid FE<br/>portpro-frontends"]
  subgraph PB["⚙️ portpro-backend — single deployment, BOTH roles"]
    BE["Hybrid/Broker BE · tms module<br/>tms-controller.js:11824<br/>getLoadFirebaseInstances"]
    DE["🏢 Drayos BE · customer-api module<br/>customer-api-controller.js:483<br/>resolveDrayosFirebaseInstances()"]
  end
  FB[("🔥 driver-location-portpro")]

  FE -->|"GET tms/load-firebase-instances"| BE
  BE ==>|"in-process call (NO HTTP)<br/>flag ON && connectDestination==DRAYOS → localMapper"| DE
  DE -.->|"[{driver, carrierId}] local ids"| BE
  BE -.-> FE
  FE -->|"onValue {ooCarrier}/currentLocation/{ooDriver}"| FB
```

> `==>` thick edge = in-process call; the **Drayos BE** here is the `customer-api` module of the **same** `portpro-backend` deployment (not a separate instance like Flow 2).

> ⚠️ **Config path is the exception (containers page only):** `getDrayosFirebaseConfig` uses `CPAHookCall(isPortproConnect)` → `connectUrl || PORTPRO_CONNECT_URL` → the **Drayos BE `customer-api` module** (`getDrayosFirebaseConfig:588`). For a same-deployment O/O, `connectUrl` points back to the same backend → returns **MAIN**. That HTTP round-trip (not the local resolve) is what breaks 3b.

---

## Detailed flow — both paths, with the Drayos-BE module

Same `portpro-backend` deployment plays **both** roles. Driver-resolve = **in-process** function call into the `customer-api` module; config = **HTTP loopback** into the same `customer-api` module.

```mermaid
flowchart LR
  subgraph FEG["🖥️ portpro-frontends"]
    ELD["EachLiveDriverWithoutELD.js"]
    LL["LoadList.js<br/>getActiveContainersFirebaseRefs()"]
    SP["useContainersTrackingSidePanel.js:159<br/>fetchDrayosFirebaseConfig() (containers)"]
    HOOK["useFirebaseRef.js:25 picker"]
    CFG["config/index.js instances"]
  end
  subgraph BEG["⚙️ portpro-backend · tms module (Hybrid BE)"]
    TMS1["tms-controller.js:11824<br/>getLoadFirebaseInstances (localMapper)"]
    TMS2["tms-controller.js:11915<br/>getDrayosFirebaseConfig"]
    SVC["customer-api-service.js:210<br/>CPAHookCall(isPortproConnect)"]
  end
  subgraph DEG["🏢 portpro-backend · customer-api module (Drayos BE — SAME deployment)"]
    RESC["customer-api-controller.js:483<br/>resolveDrayosFirebaseInstances()"]
    CTRL["customer-api-controller.js:588<br/>getDrayosFirebaseConfig → getFirebaseConfig() = MAIN"]
  end
  FBm[("🔥 driver-location-portpro (MOBILE)<br/>where O/O writes")]
  FBx[("🔥 portpro-294915 (MAIN)<br/>empty for this node")]

  LL -->|"1 · GET tms/load-firebase-instances"| TMS1
  TMS1 ==>|"2 · in-process call (NO HTTP)"| RESC
  RESC -.->|"3 · [{driver, carrierId}] local ids"| LL

  SP -->|"5 · GET tms/getDrayosFirebaseConfig (containers ONLY)"| TMS2 --> SVC
  SVC -->|"6 · HTTP connectUrl||PORTPRO_CONNECT_URL<br/>v1/broker/get-drayos-firebase-config (loops to self)"| CTRL
  CTRL -.->|"7 · config = MAIN ⚠️"| SP

  ELD --> HOOK --> CFG
  CFG -->|"3a firebaseConfig=null → mobileFirebase"| FBm
  CFG -->|"3b firebaseConfig=MAIN → getNewFirebaseInstanceByConfig"| FBx
  FBm -.->|"✅ marker"| ELD
  FBx -.->|"❌ empty"| ELD
```

> `==>` thick edge = in-process function call (driver-resolve, no network). Thin `-->` = HTTP. The Drayos-BE `customer-api` module is hit **both** ways — the config HTTP loopback is what returns MAIN.

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
```mermaid
flowchart LR
  APP["📱 owner-operator-mobile"]
  R["route /mobile/history?ownerOperator=true<br/>portpro-tracking-api · routes/index.js:58"]
  MR["mobileAuthRouter<br/>middleware/mobileAuth.js:76"]
  OO["ooAuth<br/>middleware/ooAuth.js"]
  C["trackingHistoryController"]
  M[("🍃 Mongo")]

  APP -->|"POST"| R --> MR
  MR -->|"ownerOperator==true"| OO
  OO -->|"resolve carrier + defaultDriver<br/>ownerOperatorConfig.defaultDriver"| C
  C -->|"insert history"| M
  C -->|"update currentDriverLocation.coordinates<br/>on driver RECORD"| M
```

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
