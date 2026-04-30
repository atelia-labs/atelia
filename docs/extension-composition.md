# Extension Composition

Atelia is a personal agent harness. Each workplace can become different because
extensions can add tools, memory, approval agents, external services, review
agents, delegated agents, workflows, and presentation.

That power creates conflicts. Atelia treats extension composition as a first
class product surface, not as an implementation detail.

## Composition Model

Extensions attach to surfaces. See [Extensions](extensions.md),
[Tool Output](tool-output.md), [Hooks](hooks.md), and
[Extension Security](extension-security.md) for the underlying contracts.

- tool namespace
- tool output rendering
- hook and webhook routing
- workflow routing
- approval requests
- memory / OM provider
- notification routing
- presentation
- delegated agent pool
- service namespace
- resource namespace
- client extension surface
- external service account
- branch / repository ownership
- review queue

Each attachment declares:

- scope: workspace, project, repository, branch, thread, agent, event type,
  resource namespace, or resource
- mode: exclusive, pipeline, advisory, mirror, or observer
- priority
- required capabilities
- produced state
- rollback behavior
- conflict policy

## Surface Modes

- `exclusive`: one owner at a time, such as default OM provider for a workspace
- `pipeline`: ordered chain, such as tool-output renderers
- `advisory`: multiple extensions can propose decisions, such as approval or
  review agents
- `mirror`: multiple consumers receive the same event, such as notifications
- `observer`: read-only visibility into events or state

Secretary can inspect the active composition for each surface.

## Resource Namespace

A resource namespace identifies the object area an extension touches for
composition purposes. Instead of adding domains such as finance, calendar, or
review queue as new first-class concepts, Atelia expresses them through the
existing surface / scope / mode model with `resource_namespace` and `resources`.

Example:

```yaml
composition:
  attachments:
    - surface: tool_output.categorization
      scope:
        resource_namespace: household_finance
        resources:
          - transactions
          - budgets
          - bills
      mode: exclusive
      priority: 1
```

When `exclusive` applies to a `resource_namespace` and `resources`, multiple
extensions cannot own the same target at the same time. `pipeline`, `advisory`,
`mirror`, and `observer` express supporting, notification, and observation roles
for the same namespace.

Extensions declare namespace granularity. If a namespace is too broad and
unnecessarily excludes other extensions, Atelia presents the conflict and can
suggest narrowing the scope.

## Conflict Detection

Atelia detects conflicts before enablement and during runtime.

Conflicts include:

- two extensions claim the same exclusive surface
- two approval agents can approve the same high-risk request with incompatible
  policies
- two delegated agents claim the same branch or task ownership
- two extensions claim incompatible exclusive ownership of the same resource
  namespace / resource
- two service providers claim the same service namespace with incompatible
  schemas
- two OM providers try to own the same memory namespace
- a workflow extension reroutes events already owned by another workflow
- a presentation extension hides required state
- an integration extension writes to a protected external account outside its
  declared scope

Conflict records include affected surface, extensions, scope, severity, and
available resolutions.

## Resolution

Resolution options:

- choose one extension as owner
- order extensions as a pipeline
- narrow scopes
- split resource namespaces or resources
- require human approval for a specific overlap
- keep one extension advisory-only
- disable one extension for the affected scope
- create a workplace profile with a different composition
- quarantine the extension until review

Secretary participates in the resolution. Atelia can suggest a resolution, but
the persistent composition belongs to the workplace owner and Secretary.

## Approval Aggregation

Approval agents are advisory by default. Core policy decides how submitted
approval decisions compose.

Aggregation modes:

- `single`: one authorized approval is enough
- `quorum`: N of M authorized approval agents must approve
- `human_final`: agents can recommend, human approval decides
- `deny_veto`: any authorized denial blocks the request
- `risk_split`: low risk can use agent approval; high or critical risk escalates

Each approval surface declares its aggregation mode, risk ceiling, approver
scope, expiration, and human escalation path.

## Workplace Profiles

A workplace profile is a named extension composition.

Examples:

- coding: Codex, Claude, GitHub, PR Resolve Agent, Approval Agent
- product: Linear, Slack, Claude, digest notifications, OM provider
- autonomous development: Devin, Jules, CodeRabbit, GitHub, Approval Agent
- quiet mode: minimal notifications, strict approval, no delegated agents

Profiles can be switched per workspace or project. Switching profiles creates a
composition diff and requires approval when capability surfaces change.

## Safe Mode

Safe mode starts Atelia with community extensions disabled. Official extensions
can run only in safe-mode profiles explicitly declared in their manifests:
read-only, unavailable, or diagnostic.

Safe mode keeps:

- core protocol
- extension registry inspection
- extension disable / rollback
- basic file and shell harness surfaces
- composition diff viewer

Safe mode exists so a broken extension composition never traps the workplace.

## Agent Colleagues

Extensions can add colleague agents and subordinate agents.

Agent extensions declare:

- role
- provider
- model or external service
- task scope
- tool grants
- approval behavior
- branch / artifact ownership
- reporting format
- handoff behavior
- cancellation behavior

Secretary delegates work to these agents and receives evidence, artifacts,
status, and review results through Atelia contracts.

## Approval Agents

Approval Agents are approval-specialized extensions. They review approval
requests and submit bounded decisions within human-granted scope.

They declare:

- approvable capability scope
- risk tier ceiling
- evidence requirements
- escalation rules
- denial rules
- expiration behavior
- self-change restrictions

Core approval boundaries verify the decision before execution.
