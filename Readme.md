# Docs

[Prove](https://getprove.com) is a phone verification service that allows you to utilize our API to verify your users.

Users receive an SMS or phone call and verify their phone number with pin.


## Index

* [Overview](#overview)
* [Support](#support)
* [Quick Start](#quick-start)
* [Plugin](#plugin)
* [Resources](#resources)


## Overview

1. Sign up for a free Prove [developer account](https://getprove.com/signup).

2. Grab your API test and live keys from your [dashboard](https://getprove.com/login?redirect=/).

3. Integrate with one of our RESTful API wrappers:
    * Perl <https://github.com/getprove/prove-perl>
    * Node <https://github.com/getprove/prove-node>
    * Ruby <https://github.com/getprove/prove-ruby>
    * Python <https://github.com/getprove/prove-python>
    * PHP <https://github.com/getprove/prove-php>

See [Quick Start](#quick-start) or [Plugin](#plugin) for example implementations.


## Support

Send us a tweet [@getprove](http://twitter.com/getprove).

Join us on [IRC WebChat](http://webchat.freenode.net/?channels=getprove) or with a client in `#getprove` on `irc.freenode.net`.

You can also email <support@getprove.com> or file an [Issue](https://github.com/getprove/prove-api/issues/new).


## Quick Start

1. Send us a phone number to verify by text message ("SMS").

    > Request:

    ```bash
    curl https://getprove.com/api/v1/verify \
         -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
         -d tel=1234567890
    ```

    > Response:

    ```json
    {
      "id": "518c4db62602b8fe02000061",
      "tel": "1234567890",
      "text": true,
      "call": false,
      "verified": false
    }
    ```

2. User receives SMS (text message) with a 6-digit pin.  If you're in testMode, then the pin is always `000000`.

3. Send us the user entered pin to verify their phone number.

    > Request:

    ```bash
    curl https://getprove.com/api/v1/verify/518c4db62602b8fe02000061/pin \
         -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
         -d pin=000000
    ```

    > Response:

    ```json
    {
      "id": "518c4db62602b8fe02000061",
      "tel": "1234567890",
      "text": true,
      "call": false,
      "verified": true
    }
    ```


## Plugin

Integrate our plugin within minutes.  Here's a quick example:

> ./public/index.html

```html
<form action="/verify" method="post">
  <script src="//getprove.com/v1/verify.js" data-callback="/verified.html" data-key="YOUR-API-PUBLIC-KEY" class="prove-verify"></script>
</form>
```

> ./public/verified.html

```html
<h1>Congrats, you successfully verified your phone number</h1>
```

> ./app.js

```js
var getprove = require('getprove')('YOUR-API-SECRET-KEY')
  , path     = require('path')
  , express  = require('express')
  , app      = express()

// serve up index.html file with the plugin rendered
app.use(express.static(path.join(__dirname, 'public')))

// send responses for plugin's xhr requests
app.post('/verify', function(req, res, next) {
  getprove.verify.pin(req.body.proveToken, req.body.provePin, function(err, verify) {
    if (err) return res.send(400, err.response)
    // note: here's a good spot to store verify.id to db
    res.send(200)
  })
})

// start the server
app.listen(3000)
```

Then visit <http://localhost:3000/> and test it out.


## Resources <sup>v1</sup>

Prefix all paths with `/api/v1` (e.g. `/verify` becomes `/api/v1/verify`)

**Note:** All `created` and `updated` fields are in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format as per ECMAScript 5 standard `.toISOString()` [function](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Date/toISOString).

### Verify

| Path            | Method | Description                    |
| --------------- |:------:| ------------------------------ |
| /verify         | GET    | List all verifications         |
| /verify         | POST   | Create a new verification      |
| /verify/:id/pin | POST   | Verify a pin                   |
| /verify/:id     | GET    | Retrieve existing verification |

#### List all verifications

> Request:

```bash
curl https://getprove.com/api/v1/verify
  -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
```

> Response:

```json
[
  {
    "tel": "1234567890",
    "updated": "2013-05-10T01:30:30.962Z",
    "created": "2013-05-10T01:30:30.518Z",
    "test": true,
    "verified": true,
    "call": false,
    "text": true,
    "id": "518c4db62602b8fe02000061"
  },
  {
    "tel": "1234567890",
    "updated": "2013-05-10T01:30:30.607Z",
    "created": "2013-05-10T01:30:30.606Z",
    "test": true,
    "verified": false,
    "call": false,
    "text": true,
    "id": "518c4db62602b8fe02000062"
  },
  {
    "tel": "1234567890",
    "updated": "2013-05-10T01:30:31.251Z",
    "created": "2013-05-10T01:30:30.783Z",
    "test": true,
    "verified": true,
    "call": false,
    "text": true,
    "id": "518c4db62602b8fe02000063"
  }
]
```

#### Create a new verification

> Request:

```bash
curl https://getprove.com/api/v1/verify \
     -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
     -d tel=1234567890
```

> Response:

```json
{
  "id": "518c4e762602b8fe02000061",
  "tel": "1234567890",
  "country": "US",
  "text": true,
  "call": false,
  "test": true,
  "verified": false,
  "created": "2013-05-10T01:33:42.202Z",
  "updated": "2013-05-10T01:33:42.203Z"
}
```

#### Verify a pin

> Request:

```bash
curl https://getprove.com/api/v1/verify/518c4db62602b8fe02000061/pin \
     -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
     -d pin=000000
```

> Response:

```json
{
  "id": "518c4db62602b8fe02000061",
  "tel": "1234567890",
  "text": true,
  "call": false,
  "verified": true
}
```

#### Retrieve existing verification

> Request:

```bash
curl https://getprove.com/api/v1/verify/518c4db62602b8fe02000061 \
     -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
```

> Response:

```json
{
  "id": "518c4db62602b8fe02000061",
  "tel": "1234567890",
  "text": true,
  "call": false,
  "verified": true,
  "created": "2013-05-10T01:30:30.518Z",
  "updated": "2013-05-10T01:30:30.962Z"
}
```
