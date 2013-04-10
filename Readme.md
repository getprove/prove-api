# Prove API

[Prove](http://prove.io) is a phone verification service that allows you to utilize our API to verify your users.

Users receive an SMS or phone call and verify their phone number with pin.


## Index

* [Overview](#overview)
* [Support](#support)
* [Quick Start](#quick-start)
* [Resources](#resources)


## Overview

1. Sign up for a free Prove [developer account](http://prove.io/signup).

2. Create a new app and receive your API key.

3. Integrate our RESTful API with your DSL wrapper:
    * Node (npm) <https://github.com/proveio/prove-node>
    * Ruby (gem) <https://github.com/proveio/prove-ruby> (coming soon)
    * Python (pip) <https://github.com/proveio/prove-python> (coming soon)
    * PHP (pear) <https://github.com/proveio/prove-php> (coming soon)

See [Quick Start](#quick-start) for an example implementation.


## Support

Send us a tweet [@proveio](http://twitter.com/proveio).

Join us on [IRC WebChat](http://webchat.freenode.net/?channels=prove.io) or with a client in `#prove.io` on `irc.freenode.net`.

You can also email <support@prove.io> or file an [Issue](https://github.com/proveio/prove-api/issues/new).


## Quick Start

1. Send us a phone number to verify.

    > Request:

    ```bash
    curl https://api.prove.io/v1/create \
         -u prove_testKey123: \
         -d tel=1234567890
    ```

    > Response:

    ```json
    {
      "id": "awoeif128912938",
      "tel": "1234567890",
      "pin": 819820,
      "verified": false
    }
    ```

2. User receives SMS (text message) with a 6-digit pin.

3. Send us the user entered pin to verify their phone number.

    > Request:

    ```bash
    curl https://api.prove.io/v1/verify \
         -u prove_testKey123: \
         -d tel=1234567890
         -d pin=819820
    ```

    > Response:

    ```json
    {
      "id": "awoeif128912938",
      "tel": "1234567890",
      "pin": 819820,
      "verified": true
    }
    ```
