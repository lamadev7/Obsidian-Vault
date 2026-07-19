---
title: Flow 2 — Broker → Carrier (connected carrier, cross-instance)
tags: [tracking, firebase, flow, connect, drayos-35867]
created: 2026-07-19
status: verified (code) + 1 assumed item
---

# Flow 2 — Broker → Connected Carrier

Broker has **no own drivers**. Load tendered to a **connected carrier** = separate PortPro instance (`connectDestination ≠ DRAYOS`). Broker reaches across via **Connect (CPA)** to resolve the vendor's carrier+driver and their Firebase.

Overview: [[00 - Firebase Tracking Overview]]. Contrast: [[01 - Flow 1 - Carrier to Own Driver]].

---

## Repo hop chain

```mermaid
flowchart LR
  FE["🖥️ Broker FE<br/>portpro-frontends"] -->|"tms/load-firebase-instances"| BE["⚙️ Broker BE<br/>portpro-backend<br/>getLoadFirebaseInstances"]
  BE -->|"CPAHookCall (isPortproConnect)<br/>PORTPRO_CONNECT_URL + v1/broker/*"| PA["🌐 Customer Public API<br/>portpro-public-api<br/>(Connect gateway)"]
  PA -->|"connect-x-api-key"| DE["🏢 Drayos / Vendor BE<br/>portpro-backend · customer-api<br/>getDrayosFirebaseInstances"]
  DE -.->|"[{driver, carrierId}]"| PA -.-> BE -.-> FE
```

## Sequence diagram

```mermaid
sequenceDiagram
  autonumber
  participant FE as 🖥️ Broker FE · portpro-frontends
  participant BE as ⚙️ Broker BE · portpro-backend
  participant PA as 🌐 Customer Public API · portpro-public-api
  participant DE as 🏢 Drayos BE · portpro-backend customer-api
  participant FB as 🔥 driver-location-portpro

  Note over FE,DE: 1) resolve WHICH carrier + driver
  FE->>BE: GET tms/load-firebase-instances
  BE->>PA: CPAHookCall v1/broker/load-firebase-instances<br/>(isPortproConnect → PORTPRO_CONNECT_URL)
  PA->>DE: forward (connect-x-api-key)
  DE-->>PA: [{driver, carrierId}] (vendor ids)
  PA-->>BE: same
  BE-->>FE: mapper[vendorDriver] = vendorCarrier

  rect rgba(255,180,0,0.12)
    Note over FE,DE: 2) resolve WHICH firebase — containers page ONLY
    FE->>BE: GET tms/getDrayosFirebaseConfig
    BE->>PA: CPAHookCall v1/broker/get-drayos-firebase-config
    PA->>DE: forward
    DE-->>FE: config = getFirebaseConfig() = MAIN ⚠️
  end

  Note over FE,FB: 3) subscribe
  FE->>FB: on("value") {vendorCarrier}/currentLocation/{vendorDriver}
  FB-->>FE: location payload → 🚚 marker
```

---

## 1. Resolve vendor carrier+driver (via Customer Public API)

**Hop chain:** Broker FE (`portpro-frontends`) → Broker BE (`portpro-backend`) → Customer Public API (`portpro-public-api`, `PORTPRO_CONNECT_URL`) → Drayos BE (`portpro-backend · customer-api`).

**Broker BE** — `portpro-backend/server/modules/tms/tms-controller.js getLoadFirebaseInstances:11824`
```js
isDrayosDestination = drayosCarrier.connectDestination === DRAYOS;  // FALSE (connected)
// → connectMapper branch
CPAHookCall({
  url: "v1/broker/load-firebase-instances",
  body: { tenderRefToRefNumberMapper: connectMapper },
  isPortproConnect: true,                       // → baseUrl = PORTPRO_CONNECT_URL
});
// returns [{ driver, carrierId }] in VENDOR id-space
```

**The hop target** — `portpro-backend/server/modules/customer-api/customer-api-service.js:210`
```js
const CPAHookCall = async ({ method, url, body, query, isPortproConnect = false }) => {
  const baseUrl = isPortproConnect
    ? process.env.PORTPRO_CONNECT_URL           // ← Customer Public API (portpro-public-api)
    : (process.env.CUSTOMER_WEBHOOK_URL ?? '').split('v1')[0];
  // → `${baseUrl}v1/broker/load-firebase-instances`
};
```

**Drayos/Vendor BE handler** — `customer-api-controller.js getDrayosFirebaseInstances` (route `/broker/get-drayos-firebase-instances:1252`, auth `connect-x-api-key`). Resolves the vendor's own load → `{driver, carrierId}`.

FE stores:
```js
// LoadList.js:132
brokerCarrierDriverIdMapper[vendorDriver._id] = vendorCarrierId;
```

> ⚠️ **Route-name discrepancy (unconfirmed):** CPA calls `v1/broker/load-firebase-instances`, but registered vendor route is `/broker/get-drayos-firebase-instances`. Connect gateway may rewrite — not verified.

## 2. Resolve vendor Firebase config (containers page only)

`useContainersTrackingSidePanel.js:115` → `getDrayosFirebaseConfig` → BE `getDrayosFirebaseConfig:11915` → CPA → **vendor** handler:
```js
// customer-api-controller.js:590  (VENDOR side)
const config = controllers.TmsController.getFirebaseConfig();  // = FIREBASE_DATABASEURL
return config;                                                 // ← MAIN, never mobile ⚠️
```

## 3. Namespace + instance
`EachLiveDriverWithoutELD.js:207`
```js
const carrier = brokerCarrierDriverIdMapper[driver._id];   // vendorCarrierId
if (_isBrokerContainerTrackingEnabled && carrier)
  namespace = `${vendorCarrier}/currentLocation/${vendorDriver}`;
const ref = getFirebaseRefByNameSpace({ overrideNamespace: namespace, firebaseConfig, isMobile: true });
```

**Splits by surface:**

| Surface | `firebaseConfig` | picker result | reads |
|---------|------------------|---------------|-------|
| **Load-info tab** | `null` | `:34 isMobile` | `driver-location-portpro` ✅ |
| **Containers page** | vendor **MAIN** | `:25 firebaseConfig wins` | vendor MAIN ❌ |

## 4. Subscribe + render
Same as Flow 1 — `.on("value")` → `handleDriverLocationUpdate` → `<LeafletTrackingMarker>`.

---

## Where GPS actually lives

`portpro-backend/server/modules/firebaseConnection.js:15-17`
```js
const mobileDatabase = process.env.MOBILE_FIREBASE_DATABASEURL
  ? app.database(process.env.MOBILE_FIREBASE_DATABASEURL)   // driver-location-portpro
  : database;                                               // else MAIN
```
Driver apps write GPS to the **mobile** project, not main.

```mermaid
flowchart LR
  subgraph Load-info tab
    A["firebaseConfig=null"] --> B["mobileFirebase"] --> OK(("✅ match"))
  end
  subgraph Containers page
    C["firebaseConfig=vendor MAIN"] --> D["vendor MAIN"] --> BAD(("❌ GPS is in mobile"))
  end
```

---

## Verified vs assumed
- ✅ vendor `getDrayosFirebaseConfig` → MAIN, not mobile (`customer-api-controller.js:590` + `getFirebaseConfig:828`)
- ✅ `firebaseConfig` set only by containers side-panel
- ✅ backend `mobileDatabase` = separate `MOBILE_FIREBASE_DATABASEURL` instance
- ⏳ **assumed:** `driver-location-portpro` is one org-wide project shared by connected-carrier deployments — this is what makes the load-info tab work cross-instance. Needs vendor env to confirm.

## Net
Namespace resolves correctly (vendor ids via CPA). **Load-info tab** → shared mobile DB → works. **Containers page** → vendor MAIN → marker breaks. `getDrayosFirebaseConfig` returning MAIN instead of mobile is the **same root bug** as [[03 - Flow 3 - Hybrid to OO Carrier|Flow 3b]].

## Next
- [[03 - Flow 3 - Hybrid to OO Carrier]] — DRAYOS-35867 core
