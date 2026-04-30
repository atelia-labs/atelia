# Extension Security

Custom Extensions directly affect Atelia. Atelia treats them as security- and
agency-sensitive workplace changes.

Atelia is responsible for safe installation, update, and execution of
extensions. Dangerous extensions, over-permissioned extensions, and extensions
whose behavior does not match their manifests are blocked.

The permission model should be at least as strict as Chrome extensions.

## Minimum Requirements

- permission declarations in the manifest;
- pre-install permission display and explicit approval;
- per-version signing or provenance verification;
- re-approval when an update changes permissions;
- sandboxing or capability boundaries per extension;
- separate safety models for backend and client extensions;
- fine-grained separation for repository, secret, external network,
  notification, memory, review, and workflow execution access;
- fine-grained separation for service provide / consume access;
- fine-grained separation for external event sources, webhook signatures, and
  receiving endpoints;
- explicit permission separation if browser use or computer use is introduced
  later;
- visibility and protection for trigger conditions, permissions, and execution
  history for hooks created by extensions;
- denylist / blocklist support for disabling dangerous extensions;
- audit logs and rollback to the pre-install state;
- checks that actual behavior matches the manifest.

## Permission Taxonomy

Permissions are separated at least at this granularity.

- `repo.read`
- `repo.write`
- `repo.branch.create`
- `repo.pr.create`
- `repo.merge`
- `repo.destructive`
- `secret.read:name`
- `network.none`
- `network.allowlist`
- `network.github`
- `network.connect:host`
- `network.webhook.receive:source`
- `memory.read:scope`
- `memory.write:scope`
- `service.provide:name`
- `service.call:name`
- `workflow.run`
- `workflow.run:workflow_id`
- `workflow.schedule`
- `workflow.delegate_agent`
- `notification.send:channel`
- `review.comment`
- `review.approve`
- `review.request_changes`
- `hooks.create`
- `hooks.modify`
- `hooks.receive_external:source`
- `tool_output.customize:tool_id`
- `client.view.provide`
- `client.action.run`
- `client.settings.modify`
- `browser.use`
- `computer.use`

Critical permissions require explicit human approval. Extension, hook, and agent
code has no authority to grant critical permission to itself.

## Risk Tiers

- low: read-only presentation, local formatting;
- medium: notification, memory write, external read;
- high: repository write, workflow execution, webhook, secret access;
- critical: merge, destructive repository action, broad network, agent
  delegation, browser / computer use.

Bundles do not elevate permissions automatically. Service calls between
extensions in the same bundle still require service permissions, consume /
provide declarations, and capability grants.

## Blocklist

Atelia can block extensions by:

- extension id;
- publisher;
- version range;
- artifact digest;
- signing key;
- source repository;
- permission pattern;
- vulnerability id.

The blocklist wins over local enablement. Blocked extensions stay unable to
execute hooks, receive webhooks, or run migrations.

Atelia blocks dangerous extensions before execution.
