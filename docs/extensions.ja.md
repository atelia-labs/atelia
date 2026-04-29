# Custom Extensions

Custom Extension は、ユーザーとエージェントが自分の仕事場を育てるための拡張単位です。AX だけでなく、人間にとっての UX、外部サービス連携、表示、通知、記憶、レビュー、ワークフローにも関わります。

Extension は GitHub 上で公開、導入、改善、貢献できる形にします。安全な拡張機能エコシステムのために、Atelia 本体は次の機能を持ちます。

- registry
- manifest validation
- version compatibility
- 権限表示
- 導入、更新の監査ログ
- 危険な extension の blocklist
- 権限変更時の再承認
- rollback

Atelia は extension の安全な導入、更新、実行に責任を持ちます。危険な extension、権限要求が過剰な extension、manifest と実際の挙動が一致しない extension はブロックします。

権限モデルは少なくとも Chrome 拡張機能と同等の厳格さを持つべきです。

## マニフェスト

Extension manifest は、少なくとも次の契約を表現します。

- 対応する Atelia Protocol / Atelia Secretary の version range
- 必要な権限
- 読み書きするデータ領域
- 外部ネットワーク、secret、repository、workflow 実行へのアクセス有無
- 受け取る外部 event、webhook、event source
- 通知、記憶、ワークフロー、表示、レビューへの関与
- 署名または provenance
- 失敗時の degrade behavior
- migration と compatibility note

Extension は、Secretary の判断を奪うためのものではありません。Atelia に新しい道具、表示、連携、記憶、通知、レビュー、ワークフローを追加し、人間とエージェントがより自然に働けるようにするために使います。
