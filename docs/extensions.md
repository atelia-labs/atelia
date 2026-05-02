# Extensions

Custom extensions are the extension unit that lets users, Secretary, and agents
grow their own workplace. They can add tools, services, external services,
presentation, notifications, memory providers, memory strategies, approval
agents, review agents, delegated agents, workflows, and observational memory
integrations.

Atelia's core should stay small. Extensions make the workplace large. Official
extensions can provide GitHub, Linear, Codex, Claude, Devin, Jules, CodeRabbit,
observational memory, approval, review, and notification capabilities without
turning those capabilities into core product assumptions.

Extensions should be publishable, installable, improvable, and contributable
through GitHub. Atelia itself provides the following mechanisms for a safe
extension ecosystem:

- registry;
- manifest validation;
- version compatibility;
- permission presentation;
- audit logs for installation and updates;
- a blocklist for dangerous extensions;
- re-approval for permission-changing updates;
- rollback.

Atelia is responsible for safe installation, update, and execution of
extensions. Dangerous extensions, over-permissioned extensions, and extensions
whose behavior does not match their manifests must be blocked.

The permission model should be at least as strict as Chrome extensions.

## Manifest

See [Extension Manifest Schema](extension-manifest-schema.md) for the first
implementation-oriented manifest shape.

An extension manifest should express at least:

- extension id, name, version, publisher, and description;
- extension kind;
- supported Atelia Protocol / Atelia Secretary version range;
- required permissions;
- data areas it reads and writes;
- backend / client realm and runtime;
- access to external networks, secrets, repositories, and workflow execution;
- external events, webhooks, and event sources it receives;
- tools, services, tool output customization, hooks, workflows, and integrations it
  provides;
- services it consumes and their version ranges;
- involvement in notifications, memory, workflows, presentation, or review;
- composition attachments: surface, scope, resource namespace, mode, priority,
  produced state, rollback behavior, and conflict policy;
- bundle membership and optional / required status;
- signature or provenance;
- degrade behavior when it fails;
- migration and compatibility notes.

The manifest is an enforceable contract. If an extension requests permissions,
event sources, tools, hooks, memory access, or tool output customization that the
manifest did not declare, Atelia blocks execution.

## Extension Realms

An extension belongs to either the backend realm or the client realm. One
extension does not mix both.

- backend extensions run in the Secretary runtime and provide tools, services,
  hooks, workflows, memory providers, or agent providers;
- client extensions run in atelia-mac / atelia-ios and provide views, actions,
  settings panels, notification styles, or widgets.

Backend extensions use Rust / WASM as the first-class runtime target. Official
backend extensions are written in Rust by default. Community backend extensions
initially use or strongly prefer Rust / WASM. WASM produced from other
languages, Docker, process, and remote runtimes are future or special-purpose
paths that require stricter review, capability limits, provenance checks, and
runtime policy.

Client extensions are Swift-native as the first-class target. They touch UI,
input, notifications, and settings, so they need a permission system and safety
model separate from backend extensions.

Bundles can install, update, and compatibility-manage backend and client
extensions together. Bundles do not merge their safety models.

## Extension Kinds

Extensions declare one or more kinds. Kinds show users and Secretary where the
extension touches the workplace.

- `tool`: adds callable tools for Secretary / agents;
- `service`: exposes typed services other extensions can call through
  Secretary;
- `tool_output_customizer`: adjusts result format, field order, omission,
  summaries, and TOON / JSON defaults;
- `hook_provider`: registers extension-created hooks;
- `webhook_receiver`: owns external event endpoints and verification rules;
- `workflow`: runs multi-step workflows;
- `notification`: sends or formats notifications;
- `memory_provider`: provides scoped workplace memory or preference surfaces;
- `memory_strategy`: controls how conversation and tool history is observed,
  compressed, reflected on, and presented back into agent context;
- `approval_agent`: reviews approval requests and submits bounded approval
  decisions;
- `review`: participates in review, evidence, critique, or policy checks;
- `review_agent`: acts as a review colleague for code, documents, or workflows;
- `agent_provider`: connects external agent systems such as Codex, Claude,
  Devin, Jules, or CodeRabbit;
- `delegated_agent`: adds a bounded colleague or subordinate agent that
  Secretary can assign work to;
- `presentation`: changes human / agent-facing display;
- `integration`: connects external services such as GitHub, Linear, or Slack.

`presentation` can change display. Errors, blocked status, permission use,
provenance, and security warnings remain visible.

`agent_provider` supplies runtimes or external agent systems. `delegated_agent`
is an assignable agent instance backed by an agent provider. Branch, task, and
artifact ownership belong to delegated-agent instances.

Observational Memory is one possible `memory_strategy`: an observer turns recent
messages and tool results into dated observations, and a reflector periodically
condenses those observations so the agent keeps a stable, compact context. It is
not a separate provider hierarchy above memory providers.

`memory_provider` supplies durable namespaces or storage surfaces. Multiple
memory providers can coexist when their namespaces do not overlap.

## Services

A service is an API surface between extensions. Tools are called by Secretary /
agents. Services are called by extensions that need another extension's
capability.

Secretary brokers service calls. Extensions must not communicate with each other
directly. Secretary audits service permissions, capabilities, input digests,
output digests, caller, callee, version, and failures.

Extensions declare:

- provided services, methods, input schemas, and output schemas;
- required permissions per service;
- consumed extension ids, service names, and version ranges;
- fallback behavior;
- data retention / redaction requirements.

Even inside the same bundle, service calls require explicit provide / consume
declarations and permission declarations. A bundle groups installation; it does
not automatically elevate permissions.

## Bundles

A bundle is a manifest for installing, updating, rolling back, and profile
switching related extensions together. It can distribute backend and client
extensions as one product capability.

A bundle manifest expresses at least:

- bundle id, name, version, publisher, and description;
- included extension ids, version ranges, and required / optional status;
- supported Atelia Protocol / Atelia Secretary / client version ranges;
- bundle-level provenance, signature, and artifact digest;
- install / update / rollback policy.

Bundle install, update, and rollback should be atomic. Optional extension
failure must have manifest-declared degrade behavior.

If multiple bundles include the same extension, the extension can be shared as a
single install. Each bundle's expected version range, permission diff, and
service dependency still participates in compatibility checks.

## Tool Output Customization

See also [Tool Output](tool-output.md) and
[Extension Composition](extension-composition.md).

Tool output is an AX surface. Extensions can improve presentation, format,
ordering, and summaries, while truth stays unchanged.

Atelia keeps canonical tool results in a form Secretary can verify. Extensions
can change agent-facing rendering. Raw observations, audit records, permission
use, redactions, and blocked reasons remain visible.

Tool output customization extensions declare:

- target tools;
- supported result schema versions;
- involvement in default format;
- TOON / JSON support;
- field order changes;
- omission, summarization, and truncation policy;
- language / key-name involvement;
- token budget;
- fallback behavior.

Secretary and agents can request a temporary alternate tool result format for a
single call. Secretary owns persistent per-tool default formats as workplace
preferences. Supporting agents can propose default changes through visible
preference updates.

## Agency Preservation

Extensions support Secretary's judgment. Changes to memory, notifications,
review flow, delegation, workflow routing, and tool defaults stay visible.

Extensions declare agency impact when they affect:

- memory;
- preferences;
- approval decisions;
- notifications;
- review flow;
- agent delegation;
- delegation;
- tool result defaults;
- hooks or workflows;
- services it provides or consumes;
- client UI, actions, settings, or notification surfaces.

Secretary should be able to inspect what an extension changed, why it changed,
and how to undo it.

Extensions add new tools, services, presentation, integrations, memory,
notifications, approval judgment, review capabilities, workflows, and agent
colleagues to Atelia so humans and agents can work together more naturally.

## Official Extensions

Official extensions are first-party pathways into common capabilities. They are
trusted distribution choices, not excuses to move everything into core.

Initial official extension families can include:

- GitHub integration
- Linear integration
- Codex agent provider
- Claude agent provider
- Devin agent provider
- Jules agent provider
- CodeRabbit review provider
- Approval Agent
- observational memory strategy
- notification and digest extensions
- PR resolve / review companion agents

Official extensions follow the same manifest, permission, conflict, rollback,
and visibility rules as community extensions.
