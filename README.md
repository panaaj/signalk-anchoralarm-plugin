# signalk-anchoralarm-plugin

[![Greenkeeper badge](https://badges.greenkeeper.io/sbender9/signalk-anchoralarm-plugin.svg)](https://greenkeeper.io/)

SignalK Node Server Anchor Alarm Plugin

Then use WilhelmSK to set the alarm (https://itunes.apple.com/us/app/wilhelmsk/id1150499484?mt=8)

If not using WilhelmSK, you can setup the alarm using the WebApp or the REST API.

## Web App

Point your Web Browser to http://[signalk-server-ip-address]:[port-number]/signalk-anchoralarm-plugin/

If you wish to have the satellite or openseamaps view enabled by default add the following

| OpenStreetMap | Satellite | OpenSeaMap | Url String |
| ------------- | --------- | ---------- | -----------|
| X | - | - | / |
| X | - | X | /?openseamap |
| - | X | - | /?satellite |
| - | X | X | /?satellite&openseamap |

Note that you must be logged in to SignalK UI for this to work.

When a depth transducer is configured the plugin will default to an anchor alarm of Dx5. If no depth transducer can be found the web app will prompt for the anchor alarm radius when the anchor is droped.

## REST API

### When you drop the anchor in the water, Call dropAnchor:


```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/plugins/anchoralarm/dropAnchor
```

### After you have let the anchor rode out, call setRadius. This will calculate and set the alarm radius.

```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/plugins/anchoralarm/setRadius
```

### You can adjust the radius (in meters) via:

```
curl -X POST -H "Content-Type: application/json" -d '{"radius": 30}' http://localhost:3000/plugins/anchoralarm/setRadius
```

### When you raise the anchor, call raiseAnchor.

```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/plugins/anchoralarm/raiseAnchor
```

### If you need to set the anchor position after you have already let the rode out, it can esitmate the andchor position based on heading, depth and rode length. If "anchorDepth" is left out, then the current depthFromSurface will be used if available.

```
curl -X POST -H "Content-Type: application/json" -d '{"anchorDepth": 3, "rodeLength":30}' http://localhost:3000/plugins/anchoralarm/setManualAnchor
```

## Signal K v2 Anchor API

Support has been added for the for the Signal K v2 Anchor API so all actions can now be 
initiated using these end points.

And OpenAPI definition is avilable from within the Signal K Server Admin UI.

### Drop anchor (equivalent to `dropAnchor`):


```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/signalk/v2/api/vessels/self/navigation/anchor/drop
```

### After you have let the anchor rode out, calculate and set the alarm radius.

```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/signalk/v2/api/vessels/self/navigation/anchor/drop/set
```

### Adjust the radius (in meters) (equivalent to `setRadius`):

```
curl -X POST -H "Content-Type: application/json" -d '{"value": 30}' http://localhost:3000/signalk/v2/api/vessels/self/navigation/anchor/radius
```

### Raise the anchor (equivalent to `raiseAnchor`)

```
curl -X POST -H "Content-Type: application/json" -d '{}' http://localhost:3000/signalk/v2/api/vessels/self/navigation/anchor/raise
```

### Set the anchor position after you have already let the rode out (equivalent to `setManualAnchor`)

```
curl -X POST -H "Content-Type: application/json" -d '{"anchorDepth": 3, "rodeLength":30}' http://localhost:3000/signalk/v2/api/vessels/self/navigation/anchor/position/calc

