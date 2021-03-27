# Networked-AFrame Janus Adapter

[![npm](https://img.shields.io/npm/v/naf-janus-adapter.svg)](https://www.npmjs.com/package/naf-janus-adapter)

Network adapter for [networked-aframe](https://github.com/haydenjameslee/networked-aframe) that uses the Janus WebRTC server as a backend.

This adapter was designed for [Mozilla Hubs](https://github.com/mozilla/hubs) but Hubs no longer uses it. A [community fork](https://github.com/networked-aframe/naf-janus-adapter) with updated documentation is available in the networked-aframe organization.

## Usage

naf-janus-adapter needs access to networked-aframe's `NAF` global variable. Include it **after** including networked-aframe but **before** the `networked-scene` is loaded.

## Compatibility

naf-janus-adapter should support anything that supports recent WebRTC standards (right now, the `RTPSender`-based APIs for manipulating streams and tracks). At the time of this writing, that means that many browsers (e.g. Chrome) will require the use of the [WebRTC adapter shim](https://github.com/webrtc/adapter). If you're using NPM to build your A-Frame application, you should include [webrtc-adapter](https://www.npmjs.com/package/webrtc-adapter) as a dependency of your application; if you're using the [browser distribution](https://github.com/mozilla/naf-janus-adapter/tree/master/dist), you should include the WebRTC adapter prior to including naf-janus-adapter, as shown in the example code below.

### Example

```html
<html>
<head>
  <script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
  <script src="https://aframe.io/releases/0.7.0/aframe.min.js"></script>
  <script src="https://rawgit.com/netpro2k/networked-aframe/feature/register-adapter/dist/networked-aframe.js"></script>
  <script src="https://unpkg.com/naf-janus-adapter/dist/naf-janus-adapter.min.js"></script>
</head>
<body>
   <a-scene networked-scene="
        room: 1;
        audio: true;
        adapter: janus;
        serverURL: ws://localhost:8080;
      ">
  </a-scene>
</body>
</html>
```

## Migrating to 4.0.0

4.0.0 allows the application to control when occupants are subscribed to. Occupants are no longer automatically subscribed to when they join. The application is now required to call `syncOccupants` and pass an array of `occupantId`s that it wants to subscribe to. This list should contain all `occupantsId`s that are currently desired to be subscribed to- including any that have already been subscribed to and want to continue to be so. Any occupants not on the list that are already subscribed to will be unsubscribed from. 
```js
NAF.connection.adapter.syncOccupants(arrayOfOccupantIds);
```

If you want to automatically subscribe to occupants on join, you may call `syncOccupants` with the `availableOccupants` array as the first arugument once.
```js
NAF.connection.adapter.syncOccupants(NAF.connection.adapter.availableOccupants);
```

## Development

- Dev: `npm run start`
- Build: `npm run build`
- Release: `npm run release`
