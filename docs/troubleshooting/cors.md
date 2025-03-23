---
title: CORS Errors
---

* goal
  * CORS Errors
  * 
    content="CORS errors happen in web apps if requests are made and servers don't return required headers.


## Concepts

* Cross-Origin Resource Sharing (CORS)
  * 💡== mechanism / browsers & webviews (_Example:_ ones / power Capacitor & Cordova) -- restrict -- HTTP & HTTPS requests from scripts to resources | DIFFERENT origin 💡
  * 👀if an EXTERNAL origin supports CORS & server wants to allow requests -> server -- has to send, to the browser, -- [special headers](#cors-headers) 👀 
  * 's goal
    * protect your user's data
    * prevent attacks / would compromise your app

* **origin**
  * 👀== **protocol** + **domain** + **port** 👀
  * uses
    * serve your Ionic app OR external resource
  * _Example:_
    * apps / run | Capacitor
      * origin | iOS,  `capacitor://localhost`  or 
      * origin | Android, `http://localhost` 

* if origin | your app is served (_Example:_ `ionic serve`, by default, `http://localhost:8100`) != resource's origin being requested (_Example:_ `https://api.example.com`) -> 
  * takes effect browser's [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
  * required CORS | request
    * Reason: 🧠to be made == OTHERWISE, NOT made 🧠

* CORS errors
  * use cases
    * web apps / cross-origin request is made & server does NOT return the required headers | response (== NOT CORS-enabled)
      * _Example:_ origin | your app is served : `http://localhost:8100` & resource's origin being requested : `https://api.example.com` & NO 'Access-Control-Allow-Origin' header is present | requested resource
        * -> `XMLHttpRequest` -- can NOT load -- https://api.example.com == Origin 'http://localhost:8100' NOT allowed access

## How does CORS work

### Request with preflight

* if web app -- tries to make a -- cross-origin request -> 
  * | BEFORE actual request, browser 👀-- sends a -- **preflight request** (== `OPTIONS` method) 👀
    * 
    * Reason of the preflight request: 🧠impact user data == know if 
      * EXTERNAL resource -- supports -- CORS &
      * ACTUAL request -- can be sent -- safely 🧠
    * requirements (ONLY 1! is needed) to send preflight request -- by the -- browser
      * method is
        * PUT
        * DELETE
        * CONNECT
        * OPTIONS
        * TRACE
        * PATCH OR
      * header / != 
        * Accept
        * Accept-Language
        * Content-Language
        * Content-Type
        * DPR
        * Downlink
        * Save-Data
        * Viewport-Width
        * Width OR
      * `Content-Type` header !=
        * application/x-www-form-urlencoded
        * multipart/form-data
        * text/plain OR
      * | `XMLHttpRequestUpload`, it's used
        * `ReadableStream` or
        * event listeners 

* _Example:_ we are making a `POST` request -- to -- `https://api.example.com`/ `Content-Type=application/json`
  * -> preflight request ==
    ```http
    OPTIONS / HTTP/1.1
    Host: api.example.com
    Origin: http://localhost:8100
    Access-Control-Request-Method: POST
    Access-Control-Request-Headers: Content-Type
    ```
  * if the server is CORS enabled -> 
    * parse the `Access-Control-Request-*` headers
    * `POST` request is made -- from -- `http://localhost:8100` / custom `Content-Type`
  * server -- will respond; ALLOWED origins, methods & headers; to -- preflight
    ```http
    HTTP/1.1 200 OK
    Access-Control-Allow-Origin: http://localhost:8100
    Access-Control-Allow-Methods: GET, POST, OPTIONS
    Access-Control-Allow-Headers: Content-Type
    ```
    * if returned origin & method != actual request's ones OR ANY uses headers NOT ALLOWED -> request blocked -- by the -- browser
      * == error shown | console
  * ALL `POST` requests -> -- will have a -- `Content-Type: application/json` header & ALWAYS preflighted
    * Reason: 🧠API expects JSON 🧠 

### SIMPLE requests

* 👀== requests / ALWAYS considered safe to send & NOT need a preflight 👀
* ⚠️REQUIRED conditions⚠️
  * **method**
    * GET
    * HEAD
    * POST
  * **ONLY ALLOWED headers**
    * Accept
    * Accept-Language
    * Content-Language
    * Content-Type
    * DPR
    * Downlink
    * Save-Data
    * Viewport-Width
    * Width
  * **`Content-Type` header**
    * application/x-www-form-urlencoded
    * multipart/form-data
    * text/plain
  * `XMLHttpRequestUpload` != `ReadableStream` nor event listeners 

* _Example:_ | OUR example API,
  * `GET` requests -- do NOT need to be -- preflighted
    * Reason: 🧠NO JSON data is being sent 🧠
    * -> the app -- does NOT need to use the -- `Content-Type: application/json` header
    * == ALWAYS be simple requests

## CORS Headers

### Server Headers (Response)

| Header                           | Value             | Description                                                                                                                                       |
|----------------------------------| ----------------- |---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Access-Control-Allow-Origin**  | `origin` or `*`   | == ALLOWED origin (_Example:_ `http://localhost:8100` or `*`)                                                                                     |
| **Access-Control-Allow-Methods** | `methods`         | == methods / ALLOWED \| access the resource: `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `CONNECT`, `OPTIONS`, `TRACE`, `PATCH`.                      |
| **Access-Control-Allow-Headers** | `headers`         | == \| preflight request's response / -- specify -- headers can be used \| make the ACTUAL request / ASIDE from [simple headers](#simple-requests) |
| Access-Control-Allow-Credentials | `true` or `false` | == WHETER or NOT the request -- can be made with -- credentials                                                                                   |
| Access-Control-Expose-Headers    | `headers`         | == headers / browser is ALLOWED to access                                                                                                         |
| Access-Control-Max-Age           | `seconds`         | == preflight request's results' cache time                                                                                                        |

### Browser Headers (Request)

The browser automatically sends the appropriate headers for CORS in every request to the server, including the preflight requests. 
Please note that the headers below are for reference only, and **should not be set in your app code** (the browser will ignore them).

#### All Requests

| Header     | Value    | Description                          |
| ---------- | -------- | ------------------------------------ |
| **Origin** | `origin` | Indicates the origin of the request. |

#### Preflight Requests

| Header                            | Value     | Description                                                                                       |
| --------------------------------- | --------- | ------------------------------------------------------------------------------------------------- |
| **Access-Control-Request-Method** | `method`  | Used to let the server know what method will be used when the actual request is made.             |
| Access-Control-Request-Headers    | `headers` | Used to let the server know what non-simple headers will be used when the actual request is made. |

## Solutions for CORS Errors

### A. Enabling CORS | server / you control

* if you want to enable CORS -> 
  * web server -- returns the -- [right response headers](#server-headers-response) OR
  * backend & responding to preflight requests
  * Reason: 🧠allows keep using `XMLHttpRequest`, `fetch`, OR abstractions (_Example:_ `HttpClient` | Angular)

* Ionic apps -- may be run from -- DIFFERENT origins
  * 1! origin -- can be -- specified | `Access-Control-Allow-Origin` header
    * recommendations
      * check request's header `Origin`
      * reflect `Access-Control-Allow-Origin` header | response

* ALL `Access-Control-Allow-*` headers 
  * -- have to be sent from the -- server
  * NOT belong | YOUR app code

* origins | your Ionic app -- may be served -- from

#### Capacitor

| Platform | Origin                                                                  |
| -------- |-------------------------------------------------------------------------|
| iOS      | `capacitor://localhost` <br/> if you have adjusted Capacitor config -> replace `localhost` by your OWN hostname |
| Android  | `http://localhost`                                                      |

#### Ionic WebView 3.x plugin | Cordova

| Platform | Origin               |
| -------- |----------------------|
| iOS      | `ionic://localhost` <br/> if you have adjusted Capacitor config -> replace `localhost` by your OWN hostname |
| Android  | `http://localhost`   |

#### Ionic WebView 2.x plugin | Cordova

| Platform | Origin                                                                                                      |
| -------- |-------------------------------------------------------------------------------------------------------------|
| iOS      | `http://localhost:8080` <br/> if you have adjusted Capacitor config -> replace `localhost` by your OWN port |
| Android  | `http://localhost:8080`                                                                                     |

#### Local development | browser

| Command                       | Origin                                                  |
| ----------------------------- |---------------------------------------------------------|
| `ionic serve`                 | `http://localhost:8100` or `http://YOUR_MACHINE_IP:8100` |
| `npm run start` or `ng serve` | `http://localhost:4200` \| Ionic Angular apps           |

* if you are serving multiple apps | SAME time -> port numbers can be higher 
* if you set `Access-Control-Allow-Origin: *` -> 
  * work | ALL scenarios
  * may have security implications
    * _Example:_ CSRF attacks — depending on -- how the server 
      * controls access | resources
      * use sessions & cookies

#### OTHERS
* see [here](https://enable-cors.org)
* | Express/Connect apps
  * see [cors middleware](https://github.com/expressjs/cors)
  ```javascript
  const express = require('express');
  const cors = require('cors');
  const app = express();
  
  const allowedOrigins = [
    'capacitor://localhost',
    'ionic://localhost',
    'http://localhost',
    'http://localhost:8080',
    'http://localhost:8100',
  ];
  
  // Reflect the origin if it's in the allowed list or not defined (cURL, Postman, etc.)
  const corsOptions = {
    origin: (origin, callback) => {
      if (allowedOrigins.includes(origin) || !origin) {
        callback(null, true);
      } else {
        callback(new Error('Origin not allowed by CORS'));
      }
    },
  };
  
  // Enable preflight requests for all routes
  app.options('*', cors(corsOptions));
  
  app.get('/', cors(corsOptions), (req, res, next) => {
    res.json({ message: 'This route is CORS-enabled for an allowed origin.' });
  });
  
  app.listen(3000, () => {
    console.log('CORS-enabled web server listening on port 3000');
  });
  ```

### B. How to use CORS | server / you can NOT control

#### NOT leak your keys!

* TODO:
If you are trying to connect to a 3rd-party API, first check in its documentation that is safe to use it directly from the app (client-side) and that
it won't leak any secret/private keys or credentials, as it's easy to see them in clear text in Javascript code. 
Many APIs don't support CORS on purpose, in order to force developers to use them in the server and protect important information or keys.

#### 1. Native-ONLY apps (iOS/Android)

##### Capacitor Applications (Recommended)

For Capacitor applications, use the [Capacitor HTTP API](https://capacitorjs.com/docs/apis/http). 
This API patches `fetch` and `XMLHttpRequest` to use native libraries. 
Please note that if you also deploy the application to a web-based context such as PWA or the local development server (via `ionic serve` for example) you still need to implement CORS for those scenarios.

##### Legacy Cordova Applications

For legacy Cordova applications, use the [HTTP plugin with the Awesome Cordova Plugins wrapper](https://danielsogl.gitbook.io/awesome-cordova-plugins/http). 
Please note that this plugin doesn't work in the browser, so the development and testing of the app must always be done in a device or simulator going forward.

```tsx
import { Component } from '@angular/core';
import { HTTP } from '@awesome-cordova-plugins/http/ngx';

@Component({
  selector: 'app-home',
  templateUrl: './home.page.html',
  styleUrls: ['./home.page.scss'],
})
export class HomePage {
  constructor(private http: HTTP) {}

  async getData() {
    try {
      const url = 'https://api.example.com';
      const params = {};
      const headers = {};

      const response = await this.http.get(url, params, headers);

      console.log(response.status);
      console.log(JSON.parse(response.data)); // JSON data returned by server
      console.log(response.headers);
    } catch (error) {
      console.error(error.status);
      console.error(error.error); // Error message as string
      console.error(error.headers);
    }
  }
}
```

#### 2. Native + PWAs

Send the requests through an HTTP/HTTPS proxy that bypasses them to the external resources and adds the necessary CORS headers to the responses. This proxy must be trusted or under your control, as it will be intercepting most traffic made by the app.

Also, keep in mind that the browser or webview will not receive the original HTTPS certificates but the one being sent from the proxy if it's provided. URLs may need to be rewritten in your code in order to use the proxy.

Check <a href="https://github.com/Rob--W/cors-anywhere/" target="_blank" rel="noopener">cors-anywhere</a> for a Node.js CORS proxy that can be deployed in your own server. Using free hosted CORS proxies in production is not recommended.

### C. Disabling CORS or browser web security

Please be aware that CORS exists for a reason (security of user data and to prevent attacks against your app). **It's not possible or advisable to try to disable CORS**.

Older webviews like `UIWebView` on iOS don't enforce CORS but are deprecated and are very likely to disappear soon. Modern webviews like iOS `WKWebView` or Android `WebView` (both used by Capacitor) do enforce CORS and provide huge security and performance improvements.

If you are developing a PWA or testing in the browser, using the `--disable-web-security` flag in Google Chrome or an extension to disable CORS is a really bad idea. You will be exposed to all kind of attacks, you can't ask your users to take the risk, and your app won't work once in production.

##### Sources

- <a href="https://fdezromero.com/cors-errors-in-ionic-apps" target="_blank" rel="noopener">
    CORS Errors in Ionic Apps
  </a>
- <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS" target="_blank" rel="noopener">
    MDN
  </a>
