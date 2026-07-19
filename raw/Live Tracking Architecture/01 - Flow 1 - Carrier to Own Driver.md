---
title: Flow 1 — Carrier → Own Driver (end-to-end)
tags: [tracking, firebase, flow, drayos-35867]
created: 2026-07-19
status: verified (code + data)
---

# Flow 1 — Carrier → Own Driver

Full end-to-end: screen → driver-app write → Firebase → FE read → marker render. The baseline that always works — no broker, no cross-instance, no config override.

See [[00 - Firebase Tracking Overview]] for building blocks + DB map.

## Screen
Carrier logs into TMS → **Live Tracking** (`/tracking`) or **Load → Tracking tab**. Driver runs in-house app `PortPro-remastered-flutter`, load assigned, moving.

Two pipes: **A** realtime marker (Firebase), **B** trail (Mongo).

---

## WRITE half — in-house driver app (`PortPro-remastered-flutter`)

### 1. GPS tick — `lib/ui/views/home_view/home_view_model.dart:403`
```dart
void locationChangedListener(Location location) async {
:407  if (_previousLat != location.latitude || _previousLng != location.longitude) {
:411    bearing = LocationUtils.bearingBetween(prevLat, prevLng, lat, lng);  // in-house computes bearing
:418    driverID = await _secureStorageService.getUserID();                 // driver USER _id
:419    locationData = LiveLocationData(bearing:…, lat:…, lng:…, loadID:…, referenceNumber:…, …)
:436    await _locationHistoryService.calculateTheHistoryAndSaveLocation(locationData);  // Pipe B
:438    await _firebaseService.saveLocationToFirebaseDatabase(locationData);            // Pipe A
```
In-house **always sets `bearing`** (`:420`). O/O app does not — app fingerprint.

### 2. Pipe A — Firebase write — `lib/services/firebase_service.dart:86`
```dart
:89   _locationData = {
:90     "location": [locationData.lng, locationData.lat],   // [lng, lat]
:91     "last": <ISO utc>, "status":…, "ref":…, "type":…, "prevType":…,
:96     if (bearing != null) "bearing": bearing,
      }
:104  _ref = _database.ref().child('$carrierID/currentLocation/$userID');  // node path
:116  await _ref.set(_updatedData);
```
Node = `{carrierUSERid}/currentLocation/{driverUSERid}`. `carrierID` + `userID` from secure storage (USER ids).

### 3. Which DB? — `firebase_service.dart`
```dart
:44   init():                   _database = _firebaseDatabase             // default app (google-services.json)
:33   updateFirebaseDatabase(): _database = _getOrCreateDatabase(SECONDARY)
:178  _databaseURL = _appStateService.appServer?.databaseUrl             // backend-supplied mobile DB URL
:186  Firebase.initializeApp(name:SECONDARY, databaseURL:_databaseURL)
```
`appServer.databaseUrl` = mobile DB = **`driver-location-portpro`**. In-house writes there **with bearing** (matches fresh 2026-07-13 node observed live).

### 4. Pipe B — trail to Mongo (parallel)
`home_view_model.dart:436` → `calculateTheHistoryAndSaveLocation` → app POST tracking-api `/mobile/history` → Mongo `driver_tracking_histories`. Read back over backend REST for the polyline.

---

## READ half — FE (TMS, `portpro-frontends`)

### 5. Mount — `LiveTrucksMarkersList.js`
```js
:16   function LiveTrucksMarkersList({ firebaseConfig })
:184  <EachLiveDriverWithoutELD driver={driver} firebaseConfig={firebaseConfig} />  // null on live map + load-info tab
```

### 6. Resolve namespace + instance — `EachLiveDriverWithoutELD.js:207`
```js
:208  namespace = `${currentCarrierId}/currentLocation/${driver._id}`  // own carrier + driver USER id
:210  carrier = brokerCarrierDriverIdMapper[driver._id]               // undefined (no broker)
:211  if (_isBrokerContainerTrackingEnabled && carrier)               // false → skip
:215  ref = getFirebaseRefByNameSpace({ overrideNamespace, firebaseConfig:null, isMobile:true })
```
`useFirebaseRef.js:25` firebaseConfig null → skip → `:34 isMobile` → **mobileFirebase** = `driver-location-portpro`.
**Read node == write node.** ✅

### 7. Subscribe — `EachLiveDriverWithoutELD.js:222`
```js
driverLocationRef.on("value", handleDriverLocationUpdate);
```

### 8. Marker update — `EachLiveDriverWithoutELD.js:104`
```js
:105  driverLocation = snapshot.val()
:107  driverLocation.location = location.reverse()   // [lng,lat] → [lat,lng] for Leaflet
:108  dispatch ADD_DRIVER_CURRENT_COORDINATES (state:"online", source:"mobile")
:116  setHistory(prev => [...prev, [lat,lng]])
:135-139  if stale >10min → drop (freshness guard)
```

### 9. Render — `EachLiveDriverWithoutELD.js:261`
```js
:265  history.length >= LIVE_TRACKING_LEAD_OFFSET && !initialMarker &&
:266    <LeafletTrackingMarker latlngs={history} rotate icon={truckIcon} … />  // moving, rotates by bearing
```
Trail polyline drawn separately from Mongo history (Pipe B).

---

## Full-flow one-liner

```
Driver app GPS tick (home_view_model:403)
  ├─ Pipe A: firebase_service:104  set  driver-location-portpro/{carrier}/currentLocation/{driver}  {location:[lng,lat],bearing,…}
  └─ Pipe B: tracking-api POST /mobile/history → Mongo driver_tracking_histories

TMS Live Tracking / Load-Tracking tab
  ├─ EachLive:208  namespace = {currentCarrierId}/currentLocation/{driver._id}
  ├─ picker:34     firebaseConfig null + isMobile → mobileFirebase (driver-location-portpro)   ← SAME DB
  ├─ :222          .on("value") → handleDriverLocationUpdate
  ├─ :107          [lng,lat]→[lat,lng]; setHistory
  └─ :266          <LeafletTrackingMarker> moving truck  +  trail from Mongo
```

## Why Flow 1 always works
Own driver → mapper empty → namespace stays own-carrier; `firebaseConfig` null → picker uses `mobileFirebase` — exact DB the app writes. No cross-instance, no config override. Single shared mobile DB, addressed by `{carrier}/currentLocation/{driver}`.

## Next
- Flow 2 — Broker → connected carrier (vendor Firebase via `getDrayosFirebaseConfig`)
- Flow 3 — Hybrid → OO (3a load-info OK / 3b containers BUG)
