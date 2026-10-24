# HerLife cluster configuration

This repository contains the Helm releases deployed for HerLife. The platform
repository owns the tenant boundary and Argo CD `ApplicationSet`; this
repository owns the application workloads.

The environments deployed under the `herlife` Argo CD `ApplicationSet`
(managed in the [platform](../platform/) repository):

| Environment | Chart | Release name | Host |
|---|---|---|---|
| `environments/dev` | frontend app (`app` image) | `herlife-dev-app` | app-dev-herlife.itsakila.me |
| `environments/dev-admin` | admin dashboard (`admin` image) | `herlife-dev-admin` | admin-dev-herlife.itsakila.me |
| `environments/dev-api` | backend API (`api` image) | `herlife-dev-api` | api-dev-herlife.itsakila.me |

All releases deploy into the `herlife-dev` namespace.

## Runtime configuration

All three charts pass environment through an `env` map in `values.yaml`, which
is injected into the pod containers at startup. Nothing is baked into the
images:

- **api** — reads `GIN_MODE` and `CORS_ALLOWED_ORIGINS`. The CORS list should
  contain the app and admin origin hosts.
- **app / admin** — the image's `docker-entrypoint.sh` converts `API_URL` into
  a runtime `config.js` (`window.APP_CONFIG`) that the SPA loads before its
  bundle.


## Private GHCR image

The frontend uses a private image from GHCR. Before syncing the application,
create a Docker registry pull secret named `ghcr-pull-secret` in the target
namespace using the approved secret-management process. Credentials are not
stored in this repository.
