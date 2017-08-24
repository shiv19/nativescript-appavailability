# NativeScript AppAvailability

[![NPM version][npm-image]][npm-url]
[![Downloads][downloads-image]][npm-url]
[![Twitter Follow][twitter-image]][twitter-url]

[npm-image]:http://img.shields.io/npm/v/nativescript-appavailability.svg
[npm-url]:https://npmjs.org/package/nativescript-appavailability
[downloads-image]:http://img.shields.io/npm/dm/nativescript-appavailability.svg
[twitter-image]:https://img.shields.io/twitter/follow/eddyverbruggen.svg?style=social&label=Follow%20me
[twitter-url]:https://twitter.com/eddyverbruggen

A plugin to check for availability of other apps on the device.

## Installation
Run the following command from the root of your project:

```
tns plugin add nativescript-appavailability
```

## Usage

> Note that version 1.3.0 added a synchronous version of this method that doesn't return a Promise. Need that? Use `availableSync` instead of `available`. 

### TypeScript
```typescript
const isAppAvailable = require("nativescript-appavailability").available;

// examples of what to pass:
// - for iOS: "maps://", "twitter://", "fb://"
// - for Android: "com.facebook.katana"
appavailability.available("twitter://").then((avail: boolean) => {
  console.log("App available? " + avail);
})
```

### TypeScript + Angular
```typescript
import * as appavailability from "nativescript-appavailability";

// examples of what to pass:
// - for iOS: "maps://", "twitter://", "fb://"
// - for Android: "com.facebook.katana"
appavailability.available("twitter://").then((avail: boolean) => {
  console.log("App available? " + avail);
})
```

### JavaScript

```js
var appAvailability = require("nativescript-appavailability");

// examples of what to pass:
// - for iOS: "maps://", "twitter://", "fb://"
// - for Android: "com.facebook.katana"
appAvailability.available("com.facebook.katana").then(function(avail) {
  console.log("App available? " + avail);
})
```

## Opening an app (with web fallback)
Now that you know whether an app is installed or not, you probably want to launch it.
Here's a snippet that opens the mobile Twitter app and falls back to the website if it's not installed.

```typescript
const twitterScheme = "twitter://";
available(twitterScheme).then(available => {
  if (available) {
    // open in the app
    openUrl(twitterScheme + (isIOS ? "/user?screen_name=" : "user?user_id=") + "eddyverbruggen");
  } else {
    // open in the default browser
    openUrl("https://twitter.com/eddyverbruggen");
  }
})
```


## iOS whitelisting
To get useful results on iOS 9 and up you need to whitelist the URL Scheme
you're querying in the application's `.plist`.

Luckily NativeScript made this pretty easy. Just open `app/App_ResourcesiOS/Info.plist`
and add this if you want to query for both `twitter://` and `fb://`:

```xml
  <key>LSApplicationQueriesSchemes</key>
  <array>
    <string>fb</string>
    <string>twitter</string>
  </array>
```

You may wonder how one would determine the correct identifier for an app.
* Android: simply search the Play Store and use the id in the URL. For Twitter this is com.twitter.android because the URL is https://play.google.com/store/apps/details?id=com.twitter.android.
* iOS: this one is a bit harder but this site should cover most apps you're interested in. When in doubt you can always fire up Safari on your iPhone and type for example 'twitter://' in the address bar, if the app launches you're good.
