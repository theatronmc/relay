# Theatron Relay

Theatron Relay is an optional Rust service that gives Launcher-managed Minecraft servers a public multiplayer address when the server host cannot accept inbound connections. Players connect to the relay; the Launcher agent connects outward over TLS and forwards their traffic to the local server. The relay verifies short-lived tokens signed by the selected Theatron engine.

> **Availability:** This repository currently contains a README only. Source code, a container image, and deployment instructions are not available here yet.

## Connections

The [Theatron engine](https://github.com/theatronmc/theatron) signs address tokens. The [Launcher](https://github.com/theatronmc/launcher) agent opens the outbound connection and forwards traffic to its local Paper server. The relay checks the token with the engine's public key; it does not call the engine for each connection. A server that already accepts direct player connections does not need the relay.

## Network interfaces

- **Java players:** public TCP listener; the Minecraft handshake hostname selects the destination server.
- **Bedrock players:** public UDP port allocated to a connected server.
- **Launcher agents:** TLS listener with token-authenticated address claims and a versioned control protocol.
- **Operators:** private health and metrics listener.

An operator running a relay needs a public domain, DNS and TLS configuration, exposed player ports, and the public key of the engine issuing its tokens. The engine, Launcher agent, and relay must use compatible protocol versions. The relay is optional for Theatron self-hosting.

See the [Theatron documentation](https://github.com/theatronmc/docs) for the wider product.
