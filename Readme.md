# Prove API

[Prove](https://getprove.com) is a phone verification service that allows you to utilize our API to verify your users.

Users receive an SMS or phone call and verify their phone number with pin.


## Index

* [Overview](#overview)
* [Support](#support)
* [Quick Start](#quick-start)
* [Resources](#resources)


## Overview

1. Sign up for a free Prove [developer account](https://getprove.com/signup).

2. Create a new app and receive your API key.

3. Integrate our RESTful API with your DSL wrapper:
    * Node (npm) <https://github.com/getprove/prove-node>
    * Ruby (gem) <https://github.com/getprove/prove-ruby> (coming soon)
    * Python (pip) <https://github.com/getprove/prove-python> (coming soon)
    * PHP (pear) <https://github.com/getprove/prove-php> (coming soon)

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
         -u prove_testKey123: \
         -d text=true \
         -d tel=1234567890
    ```

    > Response:

    ```json
    {
      "id": "awoeif128912938",
      "tel": "1234567890",
      "text": true,
      "call": false,
      "verified": false
    }
    ```

2. User receives SMS (text message) with a 6-digit pin.  When using a test API key, the pin is always `1337`.

3. Send us the user entered pin to verify their phone number.

    > Request:

    ```bash
    curl https://getprove.com/api/v1/verify/awoeif128912938 \
         -u prove_testKey123: \
         -d pin=1337
    ```

    > Response:

    ```json
    {
      "id": "awoeif128912938",
      "tel": "1234567890",
      "text": true,
      "call": false,
      "verified": true
    }
    ```


## Resources <sup>v1</sup>

Prefix all paths with `/api/v1` (e.g. `/verify` becomes `/api/v1/verify`)

### Verify

| Path            | Method | Description                    |
| --------------- |:------:| ------------------------------ |
| /verify         | GET    | List all verifications         |
| /verify         | POST   | Create a new verification      |
| /verify/:id/pin | POST   | Verify a pin                   |
| /verify/:id     | GET    | Retrieve existing verification |
| /verify/:id     | PUT    | Update a verification          |
| /verify/:id     | DELETE | Delete a verification          |

