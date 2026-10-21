# HerLife cluster configuration

This repository contains the Helm releases deployed for HerLife. The platform
repository owns the tenant boundary and Argo CD `ApplicationSet`; this
repository owns the application workloads.

The initial release is the frontend in `environments/dev`.

## Private GHCR image

The frontend uses a private image from GHCR. Before syncing the application,
create a Docker registry pull secret named `ghcr-pull-secret` in the target
namespace using the approved secret-management process. Credentials are not
stored in this repository.
