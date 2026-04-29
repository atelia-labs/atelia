# Governance

Atelia should be open source from the beginning, with governance that stays
small enough to maintain and explicit enough to support growth.

## Initial Governance

- `atelia-labs` owns the project.
- Maintainers are responsible for release quality, safety policy, and project
  direction.
- Contributions should be welcomed through issues, pull requests, discussions,
  design docs, and AX Feedback.
- AX Feedback for Atelia Secretary itself should be accepted through GitHub
  issues and discussions.
- AX Feedback for individual workplaces should be handled by each host's
  dedicated AX improvement agent and, when useful, grow into Custom Extensions.
- Custom Extensions should form a GitHub-based ecosystem for publishing,
  installation, improvement, and contribution.
- The extension ecosystem follows Atelia's safety policy, permission model, and
  blocklist.
- High-risk automation requires explicit review and policy support.

## Project Repositories

- `atelia`: direction, specifications, and governance
- `atelia-secretary`: backend daemon
- `atelia-kit`: shared Swift package for Apple platforms
- `atelia-mac`: macOS client
- `atelia-ios`: iOS client

## Maintainer Rule

No automation path should be able to silently grant itself more authority than
the governance documents allow.
