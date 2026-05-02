# Extension Manifest Schema

This document defines the first Atelia Custom Extension manifest shape. It is a
contract target for early implementation, not a final registry format.

The manifest is enforceable. If an extension requests permissions, tools,
services, hooks, memory access, events, presentation changes, or agent
delegation that the manifest did not declare, Atelia blocks execution.

## Format

The first manifest format is YAML.

```yaml
schema_version: atelia.extension.v0
id: com.example.github-assistant
name: GitHub Assistant
version: 0.1.0
publisher:
  id: example
  name: Example Labs
description: Adds bounded GitHub issue and pull request assistance.
realm: backend
runtime:
  kind: wasm
  language: rust
  entrypoint: github_assistant.wasm
compatibility:
  atelia_protocol: ">=0.1.0 <0.2.0"
  atelia_secretary: ">=0.1.0 <0.2.0"
kinds:
  - tool
  - service
  - agent_provider
permissions:
  - repo.read
  - network.github
  - service.provide:github.issue
data_access:
  reads:
    - repository.metadata
    - issue.metadata
  writes: []
network:
  mode: allowlist
  hosts:
    - api.github.com
tools:
  - id: github.issue.search
    schema: schemas/github.issue.search.tool.yaml
    default_output_format: toon
services:
  provides:
    - name: github.issue
      version: 0.1.0
      methods:
        - search
  consumes: []
hooks: []
events:
  receives: []
composition:
  attachments:
    - surface: agent.provider.github
      scope: workspace
      mode: shared
      priority: 50
      rollback: disable_extension
      conflict_policy: prefer_existing
agency:
  affects_memory: false
  affects_preferences: false
  affects_approval: false
  affects_notifications: false
  affects_review_flow: true
  affects_agent_delegation: true
provenance:
  source: https://github.com/example/github-assistant
  artifact_digest: sha256:example
  signature: null
degrade:
  on_unavailable: disable_extension
  user_message: GitHub assistance is unavailable.
migrations: []
```

## Required Fields

Early manifests must include:

- `schema_version`
- `id`
- `name`
- `version`
- `publisher`
- `description`
- `realm`
- `runtime`
- `compatibility`
- `kinds`
- `permissions`
- `data_access`
- `tools`
- `services`
- `hooks`
- `events`
- `composition`
- `agency`
- `provenance`
- `degrade`

## Field Groups

### Identity

`id` is globally stable and reverse-DNS-like. `name` is display text. `version`
is SemVer. `publisher` identifies the person, organization, or registry account
that produced the extension.

### Realm And Runtime

`realm` is one of:

- `backend`
- `client`

One extension does not mix realms.

Backend runtime kinds:

- `wasm`
- `process`
- `docker`
- `remote`

The first-class backend target is Rust / WASM. WASM is a host boundary and
capability boundary; it is not treated as inherently safe. Rust provides the
memory-safe default for official backend extensions.

Client runtime kinds:

- `swift`
- `native_client_bundle`

Client extensions follow client-specific review because they touch UI,
notifications, input, and settings.

### Compatibility

Compatibility declares version ranges for:

- Atelia Protocol
- Atelia Secretary
- client platform, when `realm: client`
- optional extension dependencies

Updates outside the declared range are blocked until a new manifest is approved.

### Kinds

`kinds` describes the extension's role. Valid initial values:

- `tool`
- `service`
- `tool_output_customizer`
- `hook_provider`
- `webhook_receiver`
- `workflow`
- `notification`
- `memory_provider`
- `om_provider`
- `approval_agent`
- `review`
- `review_agent`
- `agent_provider`
- `delegated_agent`
- `presentation`
- `integration`

### Permissions

`permissions` lists every requested permission. Permissions must align with
[Extension Security](extension-security.md).

Permission-changing updates require re-approval.

### Data Access

`data_access` declares what the extension reads and writes. It should name
domain areas, not implementation files, unless file scope is the actual
contract.

Examples:

- `repository.metadata`
- `repository.files:docs/**`
- `issue.metadata`
- `memory.namespace:workplace`
- `secret:name`

### Network And Secrets

`network.mode` is one of:

- `none`
- `allowlist`
- `provider`
- `broad`

`broad` is high or critical risk and needs explicit human approval.

Secrets must be declared by name or provider reference. Extensions cannot
discover arbitrary secrets.

### Tools

Tool entries declare:

- `id`
- input schema reference
- output schema reference, when distinct
- default output format
- risk tier
- required permissions
- audit fields

Tool definitions should follow the Secretary
[Tool Definition Schema](https://github.com/atelia-labs/atelia-secretary/blob/main/docs/tool-definition-schema.md).

### Services

Services are extension-to-extension API surfaces brokered by Secretary.

`services.provides` declares service names, versions, methods, schemas, and
permissions. `services.consumes` declares consumed extension ids, service names,
version ranges, and fallback behavior.

Extensions in the same bundle still need explicit provide / consume
declarations. A bundle does not automatically grant service access.

### Hooks And Events

`hooks` declares Atelia Hook surfaces the extension creates or modifies.
`events.receives` declares external event sources, webhook endpoints,
verification, replay policy, and retention.

External event changes require re-approval.

### Composition

`composition.attachments` declares where an extension attaches to the workplace.
Each attachment includes:

- `surface`
- `scope`
- `resource_namespace`
- `mode`
- `priority`
- `produced_state`
- `rollback`
- `conflict_policy`

Conflicts are resolved according to
[Extension Composition](extension-composition.md).

### Agency

`agency` declares whether the extension affects:

- memory
- preferences
- approval decisions
- notifications
- review flow
- agent delegation
- tool result defaults
- hooks or workflows
- services
- client UI / actions / settings

Agency-sensitive changes stay visible and reversible.

### Provenance

`provenance` declares source repository, artifact digest, signature, signing key,
build metadata, and registry identity where available.

Unsigned local development extensions must be visibly labeled and require
development-mode approval.

### Degrade Behavior

`degrade` declares what happens when the extension fails, is blocked, is missing
a dependency, or loses permission.

Valid initial actions:

- `disable_extension`
- `disable_feature`
- `fallback_to_builtin`
- `require_user_action`
- `block_workflow`

## Required Vs Future-Facing

Required for first implementation:

- identity
- realm/runtime
- compatibility
- kinds
- permissions
- data access
- tools/services/hooks/events lists, even when empty
- composition attachments, even when empty
- provenance
- degrade behavior

Future-facing but reserved:

- bundle membership
- migrations
- registry ranking
- pricing or license metadata
- multi-user organization policy
- remote runtime attestation
- approval-agent quorum policy

Reserved fields may be present but must not silently change execution behavior
until Atelia implements them.
