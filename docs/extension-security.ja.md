# Extension Security

Custom Extensions は、Atelia に直接影響します。Atelia は extension を security-sensitive かつ agency-sensitive な仕事場変更として扱います。

Atelia は extension の安全な導入、更新、実行に責任を持ちます。危険な extension、権限要求が過剰な extension、manifest と実際の挙動が一致しない extension はブロックします。

権限モデルは、少なくとも Chrome 拡張機能と同等の厳格さを持つべきです。

## 最低要件

- manifest による権限宣言
- 導入前の権限表示と明示的な承認
- version ごとの署名または provenance verification
- 権限変更を伴う update の再承認
- extension ごとの sandbox または capability boundary
- backend extension と client extension の別々の安全モデル
- repository、secret、外部ネットワーク、通知、記憶、review、workflow 実行への細かい権限分離
- service provide / consume への細かい権限分離
- 外部 event source、webhook 署名、受信 endpoint への細かい権限分離
- 将来 browser use / computer use を導入する場合の明示的な権限分離
- extension が作成した Hook の発火条件、権限、実行履歴の可視化と保護
- denylist / blocklist による危険な extension の無効化
- 監査ログと、導入前の状態へ戻せる rollback
- manifest と実際の挙動が一致しているかの検査

## Permission Taxonomy

権限は、少なくとも次の粒度で分離します。

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

critical permission は、人間の明示的承認を必要とします。extension、hook、agent は critical permission を自分自身へ付与できない契約にします。

## Risk Tiers

- low: read-only presentation、local formatting
- medium: notification、memory write、external read
- high: repository write、workflow execution、webhook、secret access
- critical: merge、destructive repository action、broad network、agent delegation、browser / computer use

Bundle は権限を自動昇格しません。同じ bundle 内の extension 間 service call でも、個別の service permission、consume / provide declaration、capability grant を必要とします。

## Blocklist

Atelia は、次の単位で extension を block できます。

- extension id
- publisher
- version range
- artifact digest
- signing key
- source repository
- permission pattern
- vulnerability id

blocklist は local enablement より優先します。blocked extension は hook execution、webhook receiving、migration execution の対象から外します。

危険な extension は、Atelia が実行前にブロックします。
