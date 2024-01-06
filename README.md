# Tier on Fly.io

This repository provisions tier on fly.io as a private service

## Setup

Create an app out of this template.

```sh
fly launch --no-deploy
```

Then we set the Stripe API key secret.

```sh
fly secrets set STRIPE_API_KEY=sk_live_...
```

Then we deploy it.

```sh
fly deploy
```

The app is now available internally at `http://<app>.internal/`.

## Enable public-facing access

Be very mindful about enabling public-facing access as anyone who has access to its URL can issue any command to Tier.

First, you have to allocate an IP address.

```sh
# use --shared if you donʻt want to pay $2 for a dedicated IPv4
fly ips allocate-v4 [--shared]
fly ips allocate-v6

# redeploy
fly deploy
```

## Enable scale-to-zero in private networking

You need to use [Flycast](https://fly.io/docs/reference/private-networking/#flycast-private-load-balancing) to achieve this.

```sh
# allocate a private ipv6
fly ips allocate-v6 --private

# deploy app
fly deploy

# access app via ip
curl http://[ipv6]/v1/whomai
```

You can now update the minimum machines needed by setting `min_machines_running = 0`
