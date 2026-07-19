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
  BE["⚙️ Hybrid BE<br/>portpro-backend · tms module<br/>getLoadFirebaseInstances"]
  LR2["🔁 controllers.CustomerApiController<br/>.resolveDrayosFirebaseInstances()<br/>portpro-backend · customer-api module<br/>(LOCAL in-process — no HTTP)"]
  FB[("🔥 driver-location-portpro")]

  FE -->|"GET tms/load-firebase-instances"| BE
  BE -->|"flag ON && connectDestination==DRAYOS<br/>→ localMapper (in-process call)"| LR2
  LR2 -.->|"[{driver, carrierId}] local ids"| BE
  BE -.-> FE
  FE -->|"onValue {ooCarrier}/currentLocation/{ooDriver}"| FB
```

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

## READ half — the surface split

`EachLiveDriverWithoutELD.js:207` — namespace resolves the same for both surfaces:
```js
const carrier = brokerCarrierDriverIdMapper[driver._id];   // ooCarrier (local resolve)
if (_isBrokerContainerTrackingEnabled && carrier)
  namespace = `${ooCarrier}/currentLocation/${ooDriver}`;
const ref = getFirebaseRefByNameSpace({ overrideNamespace: namespace, firebaseConfig, isMobile: true });
```

**Only `firebaseConfig` differs — and that decides everything:**

```mermaid
flowchart TD
  N["EachLiveDriverWithoutELD.js:212<br/>namespace {ooCarrier}/currentLocation/{ooDriver}"] --> Q{"useFirebaseRef.js<br/>firebaseConfig set?"}
  Q -->|"3a · LoadTrackingHistory/index.js:32<br/>firebaseConfig = null"| A["picker :34 isMobile<br/>config/index.js → mobileFirebase"]
  Q -->|"3b · useContainersTrackingSidePanel.js:159<br/>getDrayosFirebaseConfig = MAIN"| B["picker :25 firebaseConfig wins<br/>config/index.js → getNewFirebaseInstanceByConfig(MAIN)"]
  A --> AR[("🔥 driver-location-portpro<br/>= where O/O writes")]
  B --> BR[("🔥 portpro-294915<br/>MAIN — empty for this node")]
  AR --> OK(("✅ marker shows"))
  BR --> BAD(("❌ no marker"))
```

| | 3a — Load-info tab | 3b — Containers page |
|--|--------------------|----------------------|
| `firebaseConfig` | `null` (`LoadTrackingHistory:32`) | `getDrayosFirebaseConfig` = **MAIN** |
| picker | `:34 isMobile` → mobileFirebase | `:25 firebaseConfig` → MAIN |
| reads | `driver-location-portpro` ✅ | `portpro-294915` ❌ |
| result | marker shows | **no marker** |

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
