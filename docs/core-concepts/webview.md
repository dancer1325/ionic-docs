---
title: Web View
---

* Capacitor Web View | iOS & Android Apps

* Web Views
  * 💡== FULL screen + FULL-powered web browser 💡
  * 💡-- power -- web apps | native devices 💡
  * 👀if apps / integrated with [Capacitor](../reference/glossary.md#capacitor) -> Web View is AUTOMATICALLY provided 👀 
  * | [Cordova](../reference/glossary.md#cordova),
    * Ionic maintains [Web View plugin](https://github.com/ionic-team/cordova-plugin-ionic-webview)
      * if you use Ionic CLI -> provided, by default 

## What is a Web View?

* Ionic apps
  * are built -- via -- [web technologies](../reference/glossary.md#web-standards)
  * are rendered -- via -- Web Views

* 💡[Web Views features / can use hardware functionality](https://whatwebcando.today) 💡
  * -- via --
    * HTML5 APIs or
    * -- accessing -- platform-specific hardware APIs
      * | Ionic apps,
        * -- through a -- bridge layer 
          * == NATIVE plugins / expose JavaScript APIs == "bridge" between NATIVE app components -- & -- web components
          ![Web View | Ionic apps / use bridge layer](/static/img/building/webview-architecture.png)
  * _Example:_ cameras, sensors, GPS, speakers, and Bluetooth
  * Ionic Web View plugin
    * specialized | MODERN JavaScript apps
    * | iOS & Android, 
      * 💡app files -- are ALWAYS hosted, via -- `http://` protocol | optimized HTTP server / runs | LOCAL device 💡

### CORS

* [CORS](../reference/glossary.md#cors)
  * ⚠️-- enforced by -- Web Views ⚠️
    * -> external services -- properly handle -- cross-origin requests 
  * see [CORS FAQs](../troubleshooting/cors.md)

### File Protocol

* Capacitor & Cordova apps
  * 💡hosted | LOCAL HTTP server / -- served via -- `http://` protocol 💡
  * 's SOME plugins,
    * try to access, -- via `file://` protocol, to -- device files
      * ⚠️if you want to avoid difficulties `http://` & `file://` -> paths to device files -- MUST be rewritten to use -- LOCAL HTTP server ⚠️
        * _Examples:_
          * _Example1:_ `file:///path/to/device/file` -- MUST be rewritten as -- `http://<host>:<port>/<prefix>/path/to/device/file` | BEFORE rendered the app
          * _Example2:_ | Capacitor apps
            ```javascript
            import { Capacitor } from '@capacitor/core';
    
            Capacitor.convertFileSrc(filePath);
            ```
          * _Example3:_ | Cordova apps,
            * via [Ionic Web View plugin](https://github.com/ionic-team/cordova-plugin-ionic-webview), use `window.Ionic.WebView.convertFileSrc()`.
            * via Ionic Native plugin:, use [`@awesome-cordova-plugins/ionic-webview`](../native/ionic-webview.md).

### Implementations

* | iOS, [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)
* | Android, [WebView for Android](https://developer.android.com/reference/android/webkit/WebView)
