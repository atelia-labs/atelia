# Extension Security

Custom Extensions directly affect Atelia. Atelia must not treat
them as arbitrary trusted code.

Atelia is responsible for safe installation, update, and execution of
extensions. Dangerous extensions, over-permissioned extensions, and extensions
whose behavior does not match their manifests must be blocked.

The permission model should be at least as strict as Chrome extensions.

## Minimum Requirements

- permission declarations in the manifest;
- pre-install permission display and explicit approval;
- per-version signing or provenance verification;
- re-approval when an update changes permissions;
- sandboxing or capability boundaries per extension;
- fine-grained separation for repository, secret, external network,
  notification, memory, review, and workflow execution access;
- fine-grained separation for external event sources, webhook signatures, and
  receiving endpoints;
- explicit permission separation if browser use or computer use is introduced
  later;
- visibility and protection for trigger conditions, permissions, and execution
  history for hooks created by extensions;
- denylist / blocklist support for disabling dangerous extensions;
- audit logs and rollback to the pre-install state;
- checks that actual behavior matches the manifest.

Atelia should not depend on users or agents noticing danger themselves. It
should block dangerous extensions before execution.
