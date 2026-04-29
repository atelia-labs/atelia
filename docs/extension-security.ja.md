# Extension Security

Custom Extensions は、Atelia に直接影響します。Atelia は extension を安易に信頼してはいけません。

Atelia は extension の安全な導入、更新、実行に責任を持ちます。危険な extension、権限要求が過剰な extension、manifest と実際の挙動が一致しない extension はブロックします。

権限モデルは、少なくとも Chrome 拡張機能と同等の厳格さを持つべきです。

## 最低要件

- manifest による権限宣言
- 導入前の権限表示と明示的な承認
- version ごとの署名または provenance verification
- 権限変更を伴う update の再承認
- extension ごとの sandbox または capability boundary
- repository、secret、外部ネットワーク、通知、記憶、review、workflow 実行への細かい権限分離
- 外部 event source、webhook 署名、受信 endpoint への細かい権限分離
- 将来 browser use / computer use を導入する場合の明示的な権限分離
- extension が作成した Hook の発火条件、権限、実行履歴の可視化と保護
- denylist / blocklist による危険な extension の無効化
- 監査ログと、導入前の状態へ戻せる rollback
- manifest と実際の挙動が一致しているかの検査

ユーザーやエージェントが危険性を見抜けるとは限りません。危険な extension は、Atelia が実行前にブロックします。
