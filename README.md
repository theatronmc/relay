# Theatron Relay

Theatron Relay is an optional Rust service for giving Launcher-managed Minecraft servers a public multiplayer address when the server host cannot accept inbound connections. A player connects to the relay; the Launcher agent connects outward over TLS and forwards that player's traffic to the local server. The relay verifies a short-lived token signed by the selected Theatron engine. It does not call Apeyron Central to route a connection.

> **Status — preparing the source release.** This repository currently contains scope documentation only. The Rust implementation, container image, deployment guide, license and release artifacts are not published here yet. There is no installable release from this repository today.

## Why a separate repository

The relay is a standalone network service with its own Rust build, container, tests, operational controls and release cadence. It is not required for a directly reachable Paper server or for the first self-hosted Theatron installation. Apeyron Cloud will operate its own relay deployment; a self-hoster may later run a compatible relay using their own domain, DNS, TLS certificates, ports and signing-key trust.

The resource-pack build worker is different: it runs in a separate container but currently imports the core engine's repositories, entities and publication services, while its compiler build consumes the core's plugin targets and shared fixtures. Its source and image release belong with the [core repository](https://github.com/theatronmc/theatron) until a stable independent contract exists.

## Intended source release

- Rust service, lockfile, tests and independently buildable container image.
- Operator guide for public Java TCP and Bedrock UDP listeners, private health and metrics endpoints, DNS/TLS, limits, abuse response, upgrades and shutdown.
- Versioned agent ↔ relay protocol and token contract, with shared test vectors and a compatibility matrix for the [Launcher](https://github.com/theatronmc/launcher) and engine.
- Reproducible conformance and integration checks across the repository boundary.
- Required upstream attribution, notices and license files. The current private implementation derives from `vgskye/e4mc-quiclime`; those obligations will be reviewed and carried into the source release.

The present implementation lives privately under `apps/relay` in Theatron's development monorepo. Its protocol mirrors types in the core's shared package and currently reads a core-owned fixture. Before extracting source, we will move or version that fixture, prove standalone builds, test interoperability with pinned core and Launcher releases, and review source and dependencies for publication.

See the [core repository](https://github.com/theatronmc/theatron) and [documentation](https://github.com/theatronmc/docs) for the broader open-source release status.
