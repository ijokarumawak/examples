# Vouch-proxy, Nginx and Auth0 example

This example is created based on the great post, [SSO for your App via Auth0 + Nginx + Docker + Vouch-Proxy](https://sebastianwallkoetter.wordpress.com/2020/11/01/sso-for-your-app/).

I followed the post carefully, however, could not make it work as expected. It turned out that the latest Vouch docker image uses v0.27.1 (as of 2022 Oct 5), which does not support running Vouch under a Nginx location. Since the tutorial uses `/sso`, it only works with Vouch v0.33.0 or later, that supports [`vouch: document_root`](https://github.com/vouch/vouch-proxy#vouch-proxy-in-a-path). The tutorial expects a authenticaiton result is redirected to `/sso/auth`, but it goes to `/auth` and then 404 result eventually.

So, I tested with the set of configurations in this Gist, running Vouch under root path `/`. Hoping this will be helpful.

## How to configure Auth0

Login to your Auth0 page, create an application. Set Application URIs as follows:

- Allowed Callback URLs: `http://localhost/auth`
- Allowed Logout URLs: `http://localhost/,http://localhost/validate`

Find following values at "Basic Information" at the top of the page:

- Domain
- Client ID
- Client Secret

## How to configure Vouch
Update `vouch/config.yml` file with your Auth0 values. The file has configurations requiring change with `{change_with_xxx}` place holders.

## How to configreu Nginx
Update `nginx/conf.d/server.conf` file with your Auth0 values. The file has configurations requiring change with `{change_with_xxx}` place holders.

## How to run

```bash
docker-compose up
```

Then, access `localhost` from your web browser.


