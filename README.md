[![DOKKU](https://img.shields.io/badge/DOKKU->=0.4.x-success.svg)](http://dokku.viewdocs.io/dokku/development/plugin-creation/)
[![WIP](https://img.shields.io/badge/WIP-critical.svg)](#WIP)

# dokku-nginx-vhost-trustproxy

If your deployment architecture is `proxy* -> dokku-host -> app` then you most probably want the client's 
remote IP within the app to actually be the correct client IP and not the IP of the known `proxy`. 
This enables allowing an application's dokku-host nginx-vhost to forward the correct headers.

## Installation

    # on 0.4.x+
    sudo dokku plugin:install https://github.com/kingsquare/dokku-nginx-vhost-trustproxy.git

## Commands

|Command|Description|
|---|---|
|`nginx-vhost-trustproxy:enable <app> [depth]`|Trust the nth hop from the dokku host as the client IP. Default is 1 so `proxy -> dokku-host -> app`. For each extra proxy increase the depth|
|`nginx-vhost-trustproxy:disable <app>`|Disable trustproxy *This reverts to the default dokku behaviour.*|
|`nginx-vhost-trustproxy:status <app>`|Get trustproxy status|

## Credits

This is slightly based on the reasoning for [expressjs's ](https://expressjs.com/en/guide/behind-proxies.html) `trust proxy` setting.

## WIP

Do not use this in production environments

 * [ ] Enforce the depth/level on the proxy header, currently it allows all (do not use this in production)
