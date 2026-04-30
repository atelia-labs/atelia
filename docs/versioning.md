# Versioning and Releases

Atelia uses separate version streams for each implementation repository.

## Version Streams

- `atelia`: project-wide docs and specifications. Releases are milestone-based
  and can use dated tags when useful.
- `atelia-secretary`: Rust daemon. Uses SemVer.
- `atelia-kit`: shared Swift package. Uses SemVer.
- `atelia-mac`: native macOS client. Uses SemVer once installable app builds
  exist.
- `atelia-ios`: native iOS client. Uses SemVer once installable app builds
  exist.

## Pre-1.0 Rule

Before `1.0.0`, breaking changes are allowed in minor versions, but they must be
called out in release notes.

Patch versions should remain compatible within the same minor version.

## Public Contracts

The following surfaces are public contracts once referenced by an implementation
repository or extension author:

- extension manifest schema;
- permission taxonomy and risk tiers;
- hook and webhook schemas;
- tool-output result schemas and status codes;
- Atelia Protocol APIs and generated client artifacts;
- registry, provenance, and blocklist metadata;
- compatibility ranges declared by extensions and clients.

## Change Classification

| Change | Release handling |
| --- | --- |
| Add optional manifest fields, optional hook fields, optional tool-result fields, or new permissions unused by existing extensions | compatible change |
| Add enum values with documented fallback behavior | compatible change |
| Rename or remove fields, change field meaning, change status semantics, or change fallback behavior | breaking note in release notes; pre-1.0 minor bump |
| Tighten permission requirements, change default tool-output format, change field order relied on by agents, or change hook verification behavior | breaking note and migration note when needed |
| Change manifest schema version, permission model, persisted extension state, registry metadata, or tool-output major schema | migration note and compatibility-range update |

## Tags

Each repository owns its own tags.

- implementation repositories use `vMAJOR.MINOR.PATCH`;
- project-wide documentation tags can use `vMAJOR.MINOR.PATCH` or dated tags
  when they describe a governance/specification snapshot.

## Release Notes

Every release should include:

- summary;
- breaking changes;
- migration notes;
- security notes;
- contributors;
- links to important issues and pull requests.

## Initial Versions

Initial public development starts at `0.1.0` once a repository has a usable
release artifact or documented API surface.
