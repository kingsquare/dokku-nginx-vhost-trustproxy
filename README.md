[![DOKKU](https://img.shields.io/badge/DOKKU->=0.4.x-success.svg)](http://dokku.viewdocs.io/dokku/development/plugin-creation/)
[Balzing Fast](https://img.shields.io/badge/Blazing-Fast-success.svg)

# dokku-nginx-vhost-trustproxy

If your deployment architecture is `proxy* -> dokku-host -> app` then you most probably want the client's 
remote IP within the app to actually be the correct client IP and not the IP of the known `proxy`. 
This enables allowing an application's dokku-host nginx-vhost to forward the correct headers (`X-Forwarded-*`) from the [upstream proxy](http://tools.ietf.org/html/rfc2616#section-1.3) to the downstream app.

## Installation

    # on 0.4.x+
    sudo dokku plugin:install https://github.com/kingsquare/dokku-nginx-vhost-trustproxy.git

## Commands

|Command|Description|
|---|---|
|`nginx-vhost-trustproxy:enable <app> [depth]`|Trust the nth hop from the dokku host as the client IP. Default is 1 so `proxy -> dokku-host -> app`. For each extra proxy increase the depth. This sets an environment proroperty `NGINX_VHOST_TRUSTPROXY` with the depth|
|`nginx-vhost-trustproxy:disable <app>`|Disable trustproxy *This reverts to the default dokku behaviour.*|
|`nginx-vhost-trustproxy:status <app>`|Get trustproxy status|

## Application usage

Your app will be able to access the environment `NGINX_VHOST_TRUSTPROXY`. The value will be the trusted depth (including the dokku host). The left most IP according to your depth should be used as the Client/Remote IP.

### node / express
In your application you will have to use some kind of `trust proxy` to use the "correct" IP as the client IP. For example the `trust proxy` setting in [expressjs](https://expressjs.com/en/guide/behind-proxies.html). If you are not using express see the implementation in [proxy-addr](https://www.npmjs.com/package/proxy-addr). For other technologies/languages/platforms see their relevant documentation.


## Credits

This is slightly based on the reasoning for [expressjs's ](https://expressjs.com/en/guide/behind-proxies.html) `trust proxy` setting.

