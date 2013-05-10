# Docs

[Prove](https://getprove.com) is a phone verification service that allows you to utilize our API to verify your users.

Users receive an SMS or phone call and verify their phone number with pin.


## Index

* [Overview](#overview)
* [Support](#support)
* [Quick Start](#quick-start)
* [Resources](#resources)


## Overview

1. Sign up for a free Prove [developer account](https://getprove.com/signup).

2. Grab your API test and live keys from your [dashboard](https://getprove.com/login?redirect=/).

3. Integrate our RESTful API with your DSL wrapper:
    * Perl <https://github.com/getprove/prove-php> (coming soon)
    * Node <https://github.com/getprove/prove-node>
    * Ruby <https://github.com/getprove/prove-ruby>
    * Python <https://github.com/getprove/prove-python>
    * PHP <https://github.com/getprove/prove-php>

See [Quick Start](#quick-start) for an example implementation.


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

2. User receives SMS (text message) with a 6-digit pin.  If you're in testMode, then the pin is always `1337`.

3. Send us the user entered pin to verify their phone number.

    > Request:

    ```bash
    curl https://getprove.com/api/v1/verify/518c4db62602b8fe02000061/pin \
         -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
         -d pin=1337
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

```bash
[
  {
    tel: '1234567890',
    updated: '2013-05-10T01:30:30.962Z',
    created: '2013-05-10T01:30:30.518Z',
    test: true,
    verified: true,
    call: false,
    text: true,
    id: '518c4db62602b8fe02000061'
  },
  {
    tel: '1234567890',
    updated: '2013-05-10T01:30:30.607Z',
    created: '2013-05-10T01:30:30.606Z',
    test: true,
    verified: false,
    call: false,
    text: true,
    id: '518c4db62602b8fe02000062'
  },
  {
    tel: '1234567890',
    updated: '2013-05-10T01:30:31.251Z',
    created: '2013-05-10T01:30:30.783Z',
    test: true,
    verified: true,
    call: false,
    text: true,
    id: '518c4db62602b8fe02000063'
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
  id: '518c4e762602b8fe02000061',
  tel: '1234567890',
  country: 'US',
  text: true,
  call: false,
  test: true,
  verified: false,
  created: '2013-05-10T01:33:42.202Z',
  updated: '2013-05-10T01:33:42.203Z'
}
```

#### Verify a pin

> Request:

```bash
curl https://getprove.com/api/v1/verify/518c4db62602b8fe02000061/pin \
     -u test_74Lw5SoskNxT4aZmN9kZUw7ykzx: \
     -d pin=1337
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
  id: '518c4db62602b8fe02000061',
  tel: '1234567890',
  text: true,
  call: false,
  verified: true,
  created: '2013-05-10T01:30:30.518Z',
  updated: '2013-05-10T01:30:30.962Z'
}
```
