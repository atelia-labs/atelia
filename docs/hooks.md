# Hooks

A hook reacts to events in Atelia or external events such as GitHub webhooks,
and can run an automation, workflow, notification, memory update, or action by
an extension.

Hooks should be configurable directly by users, without requiring a plugin or
extension.

User-created hooks and extension-created hooks should be shown separately. Hooks
created by extensions must be visible in a protected state.

The hook surface includes:

- creating user / extension / automation;
- trigger condition, target event, and event source;
- verification method for external events;
- required permissions and capabilities;
- action to execute;
- status such as enabled, disabled, blocked, or needs approval;
- recent execution history, changed state, and failure reasons;
- why Atelia blocked it.

A hook can be enabled only when its manifest matches its actual behavior.
Permission changes or trigger-condition changes require re-approval.

## Hook Kinds

- user-created hook;
- extension-created hook;
- system hook;
- scheduled hook;
- external webhook hook;
- workflow continuation hook.

## Webhook Contract

External webhook hooks declare at least:

- allowed source;
- event schema version;
- signature verification method;
- timestamp / replay window;
- delivery id dedupe;
- secret rotation expectations;
- failure policy;
- rate limit;
- disabled / blocked behavior.

Events that fail signature verification stay in `unverified` or `failed` state
with action execution disabled.

## Agency Preservation

Hooks support Secretary's judgment. Even when an event triggers an action,
Secretary should be able to inspect, pause, disable, and reroute hooks.

Extension-created hooks show trigger conditions, permissions, execution history,
and changed state so workflow routing and memory updates stay visible.
