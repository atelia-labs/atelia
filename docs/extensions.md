# Custom Extensions

Custom extensions are the extension unit that lets users and agents grow their
own workplace. They affect not only AX, but also human UX, external service
integrations, presentation, notifications, memory, review, and workflows.

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

An extension manifest should express at least:

- supported Atelia Protocol / Atelia Secretary version range;
- required permissions;
- data areas it reads and writes;
- access to external networks, secrets, repositories, and workflow execution;
- external events, webhooks, and event sources it receives;
- involvement in notifications, memory, workflows, presentation, or review;
- signature or provenance;
- degrade behavior when it fails;
- migration and compatibility notes.

Extensions are not a way to remove Secretary's judgment. They add new tools,
presentation, integrations, memory, notifications, review capabilities, and
workflows to Atelia so humans and agents can work together more naturally.
