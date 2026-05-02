# Atelia

[日本語版 README](README.ja.md)

Atelia is your own agent harness, made by you, for you, and only for you.

Atelia gives Secretary a personal, extensible multi-agent workplace. Secretary
orchestrates tools, approval agents, memory providers, external services, and
agent colleagues through a small core and a large extension surface.

Atelia aims for MDP: a Minimum Desirable / Delightful Product that is small,
worth wanting, trustworthy, and pleasant to return to from the beginning.

This repository is the home for Atelia's product concept, vocabulary, AX
principles, extension specifications, and governance. Implementations live in separate
repositories with clearer ownership boundaries.

## Project Family

- `atelia`: philosophy, specifications, and governance for the whole project
- `atelia-secretary`: Rust backend daemon
- `atelia-kit`: shared Swift package for Apple platforms
- `atelia-mac`: macOS client
- `atelia-ios`: iOS client

## Docs

- [Project Charter](docs/charter.md)
- [Atelia Philosophy](docs/philosophy/atelia.md)
- [Agent Experience](docs/agent-experience.md)
- [AX Feedback](docs/ax-feedback.md)
- [Tool Output](docs/tool-output.md)
- [Client UX](docs/client-ux.md)
- [Extensions](docs/extensions.md)
- [Extension Manifest Schema](docs/extension-manifest-schema.md)
- [Extension Composition](docs/extension-composition.md)
- [Extension Security](docs/extension-security.md)
- [Hooks](docs/hooks.md)
- [Versioning and Releases](docs/versioning.md)
- [Governance](docs/governance.md)
