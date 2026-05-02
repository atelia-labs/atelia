# Extension Manifest Schema

この文書は、Atelia Custom Extension の最初の manifest shape を定義します。これは初期実装のための contract target であり、最終的な registry format ではありません。

manifest は Atelia が enforce する contract です。manifest が宣言していない
permission、tool、service、hook、memory access、event、presentation change、
agent delegation を extension が要求した場合、Atelia は実行を block します。

## Format

最初の manifest format は YAML です。

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

初期 manifest は次を含めます。

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

`id` は global に安定した reverse-DNS 風の識別子です。`name` は表示名です。`version` は SemVer です。`publisher` は extension を作った個人、組織、registry account を識別します。

### Realm And Runtime

`realm` は次のいずれかです。

- `backend`
- `client`

1つの extension に realm を混ぜません。

Backend runtime kind:

- `wasm`
- `process`
- `docker`
- `remote`

backend の第一級 target は Rust / WASM です。WASM は host boundary と
capability boundary であり、それ自体を inherently safe とは扱いません。
Official backend extension は Rust の memory-safe default を使います。

Client runtime kind:

- `swift`
- `native_client_bundle`

Client extension は UI、notification、input、settings に触れるため、client-specific review に従います。

### Compatibility

Compatibility は次の version range を宣言します。

- Atelia Protocol
- Atelia Secretary
- `realm: client` の場合は client platform
- optional extension dependencies

宣言範囲外の update は、新しい manifest が承認されるまで block します。

### Kinds

`kinds` は extension の役割を表します。初期値:

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

`permissions` は要求するすべての permission を列挙します。Permission は [Extension Security](extension-security.ja.md) と整合させます。

permission-changing update は再承認を要求します。

### Data Access

`data_access` は extension が読むもの、書くものを宣言します。実装ファイル名ではなく domain area を指定します。ただし file scope 自体が contract である場合は file scope を使います。

例:

- `repository.metadata`
- `repository.files:docs/**`
- `issue.metadata`
- `memory.namespace:workplace`
- `secret:name`

### Network And Secrets

`network.mode` は次のいずれかです。

- `none`
- `allowlist`
- `provider`
- `broad`

`broad` は high または critical risk であり、明示的な human approval を必要とします。

Secret は名前または provider reference で宣言します。Extension は任意の secret を discovery できません。

### Tools

Tool entry は次を宣言します。

- `id`
- input schema reference
- distinct な場合は output schema reference
- default output format
- risk tier
- required permissions
- audit fields

Tool definition は Secretary の
[Tool Definition Schema](https://github.com/atelia-labs/atelia-secretary/blob/main/docs/tool-definition-schema.ja.md)
に従います。

### Services

Service は Secretary が仲介する extension-to-extension API surface です。

`services.provides` は service name、version、method、schema、permission を宣言します。
`services.consumes` は consumed extension id、service name、version range、
fallback behavior を宣言します。

同じ bundle 内の extension でも、明示的な provide / consume 宣言が必要です。Bundle は service access を自動 grant しません。

### Hooks And Events

`hooks` は extension が作成または変更する Atelia Hook surface を宣言します。
`events.receives` は external event source、webhook endpoint、verification、
replay policy、retention を宣言します。

external event の変更は再承認を必要とします。

### Composition

`composition.attachments` は extension が仕事場のどこへ attach するかを宣言します。各 attachment は次を含みます。

- `surface`
- `scope`
- `resource_namespace`
- `mode`
- `priority`
- `produced_state`
- `rollback`
- `conflict_policy`

Conflict は [Extension Composition](extension-composition.ja.md) に従って解決します。

### Agency

`agency` は extension が次に影響するかを宣言します。

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

agency-sensitive な変更は visible かつ reversible に保ちます。

### Provenance

`provenance` は source repository、artifact digest、signature、signing key、build metadata、registry identity を可能な範囲で宣言します。

署名されていない local development extension は visible に label し、development-mode approval を必要とします。

### Degrade Behavior

`degrade` は extension が失敗した場合、block された場合、dependency を失った場合、permission を失った場合の挙動を宣言します。

初期 action:

- `disable_extension`
- `disable_feature`
- `fallback_to_builtin`
- `require_user_action`
- `block_workflow`

## Required Vs Future-Facing

最初の実装で required:

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

future-facing だが予約するもの:

- bundle membership
- migrations
- registry ranking
- pricing or license metadata
- multi-user organization policy
- remote runtime attestation
- approval-agent quorum policy

reserved field は存在しても構いません。ただし Atelia が実装するまでは、execution behavior を黙って変えてはいけません。
