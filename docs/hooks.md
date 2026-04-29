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
