---
title: Flow 2 — Broker → Carrier (connected carrier, cross-instance)
tags: [tracking, firebase, flow, connect, drayos-35867]
created: 2026-07-19
status: verified (code) + one assumed item (shared mobile project)
---

# Flow 2 — Broker → Carrier (connected carrier)

Broker has **no own drivers**. Load tendered to a **connected carrier** = separate PortPro instance, `connectDestination ≠ DRAYOS`. Broker reaches across via Connect to (a) resolve which carrier+driver, (b) learn which Firebase holds their GPS.

See [[00 - Firebase Tracking Overview]] for building blocks + DB map. Contrast [[01 - Flow 1 - Carrier to Own Driver]].

## Screen
Broker **Live Tracking** / **Load → Tracking tab** — wants connected carrier's driver moving on map. Broker never had that driver locally.

---

## 1. Resolve vendor carrier+driver — via Connect (CPA)

`portpro-backend tms-controller.js getLoadFirebaseInstances` (`:11824`)
```js
:11875  isDrayosDestination = drayosCarrier.connectDestination === DRAYOS   // FALSE (connected)
:11878  → connectMapper[key]                                               // else branch
:11886  connectMapper non-empty:
:11887    CPAHookCall({ url:"v1/broker/load-firebase-instances",
:11890                  body:{ tenderRefToRefNumberMapper: connectMapper },
:11892                  isPortproConnect:true })                           // hop to VENDOR instance
:11899  data = vendor rows → [{ driver, carrierId }]   (VENDOR id-space)
```
Vendor handler `customer-api-controller.js getDrayosFirebaseInstances` (route `/broker/get-drayos-firebase-instances:1252`, auth `connect-x-api-key`) resolves its own load → `{driver, carrierId}` in **vendor** ids.

FE stores: `LoadList.js:132` `brokerCarrierDriverIdMapper[vendorDriver._id] = vendorCarrierId`.

> ⚠️ **Discrepancy flagged:** CPA calls `v1/broker/load-firebase-instances` but registered vendor route is `/broker/get-drayos-firebase-instances`. Either Connect gateway rewrites, or worth verifying. Rewrite NOT confirmed.

## 2. Resolve vendor Firebase config — `getDrayosFirebaseConfig`

Only **containers side-panel** fetches it: `useContainersTrackingSidePanel.js:115,159` → `getDrayosFirebaseConfig` → BE `tms-controller.js getDrayosFirebaseConfig:11915` → CPA `v1/broker/get-drayos-firebase-config` → **vendor** handler:
```js
customer-api-controller.js:590  const config = controllers.TmsController.getFirebaseConfig();  // ← MAIN
:591  return config;   // FIREBASE_DATABASEURL, NOT mobile
```
→ `firebaseConfig` = **vendor MAIN** (portpro-294915-equivalent), **never the mobile GPS DB.**

## 3. Namespace + instance — `EachLiveDriverWithoutELD.js:207`
```js
:210  carrier = brokerCarrierDriverIdMapper[driver._id]      // vendorCarrierId
:211  _isBrokerContainerTrackingEnabled && carrier → TRUE
:212  namespace = `${vendorCarrier}/currentLocation/${vendorDriver}`
:215  ref = getFirebaseRefByNameSpace({ overrideNamespace, firebaseConfig, isMobile:true })
```

Instance splits by surface (`useFirebaseRef.js`):

| Surface | `firebaseConfig` | picker line | Instance read |
|---------|------------------|-------------|---------------|
| **Load-info tab** | `null` (`LoadTrackingHistory:32`, store never set here) | `:34 isMobile` | **mobileFirebase** = `driver-location-portpro` |
| **Containers page** | vendor **MAIN** (`getDrayosFirebaseConfig`) | `:25 firebaseConfig wins` | **vendor MAIN** (ignores isMobile) |

## 4. Subscribe + render
Same as Flow 1 — `:222 .on("value")` → `handleDriverLocationUpdate:104` → `setHistory` → `<LeafletTrackingMarker>` (`:266`).

---

## Where GPS actually lives + who's right

`firebaseConnection.js:15-17` — every deployment backend has `mobileDatabase = MOBILE_FIREBASE_DATABASEURL`. Driver apps write GPS to that **mobile** project (`driver-location-portpro`), NOT main.

- **Load-info tab** reads `driver-location-portpro` at `{vendorCarrier}/currentLocation/{vendorDriver}`. Works **IF** `driver-location-portpro` is the shared org-wide mobile project (vendor app writes there). Same DB as write → ✅
- **Containers page** reads vendor **MAIN** — GPS is in mobile → **MISMATCH → no marker.** ❌ Same bug class as 3b.

## Verified vs assumed
- ✅ `getDrayosFirebaseConfig` → vendor MAIN, not mobile (`customer-api-controller.js:590` + `getFirebaseConfig:828`)
- ✅ `firebaseConfig` set only by containers side-panel; load-info tab = null
- ✅ backend `mobileDatabase` = separate `MOBILE_FIREBASE_DATABASEURL` instance (`firebaseConnection.js:15`)
- ⏳ **Assumed:** `driver-location-portpro` is one org-wide project shared by connected-carrier deployments — what makes load-info tab work cross-instance. Not directly verified (needs vendor env).

## Net
Broker→connected: **namespace** correct (vendor ids via CPA). **Load-info tab** reads shared mobile DB → works. **Containers page** reads vendor MAIN → marker breaks. `getDrayosFirebaseConfig` returning MAIN instead of mobile is the **same root bug** hitting both connected-carrier and O/O on the containers surface.

## Next
- [[03 - Flow 3 - Hybrid to OO Carrier]] — 3a load-info OK / 3b containers BUG (DRAYOS-35867 core)
