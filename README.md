# Atelia

[日本語版 README](README.ja.md)

Atelia is a workplace for AI agents.

The Atelia project treats AI agents as end users who work inside Atelia. Its goal
is to give AI consistent memory and personality so Secretary and supporting
agents can grow with experience. By treating AI like a human being, Atelia draws
out AI's full potential.

This repository is the home for Atelia's philosophy, vocabulary, AX principles,
extension specifications, and governance. Implementations live in separate
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
- [Client UX](docs/client-ux.md)
- [Custom Extensions](docs/extensions.md)
- [Extension Security](docs/extension-security.md)
- [Hooks](docs/hooks.md)
- [Governance](docs/governance.md)
