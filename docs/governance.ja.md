# Governance

Atelia は最初から open source であるべきです。ガバナンスは、維持できる範囲で小さく、成長に必要な分だけ明示的であるべきです。

## 初期ガバナンス

- `atelia-labs` がプロジェクトを所有します
- maintainer は、リリース品質、安全ポリシー、プロジェクトの方向性に責任を持ちます
- issue、pull request、discussion、design doc、AX Feedback を通じて貢献できます
- Atelia Secretary 本体への AX Feedback は、GitHub issue / discussion として扱います
- 個別の仕事場への AX Feedback は、各ホストの AX 改善専任エージェントが扱い、必要に応じて Custom Extension として育てます
- Custom Extensions は GitHub で公開、導入、改善、貢献できるエコシステムとして扱います
- 拡張機能エコシステムは、Atelia の安全ポリシー、権限モデル、blocklist に従います
- 高リスクな自動化には、明示的な review と policy support が必要です

## プロジェクトリポジトリ

- `atelia`: 方針、仕様、ガバナンス
- `atelia-secretary`: backend daemon
- `atelia-kit`: Apple platform 向け共有 Swift package
- `atelia-mac`: macOS client
- `atelia-ios`: iOS client

## メンテナーの原則

どの自動化経路も、ガバナンス文書が許している以上の権限を、黙って自分自身に与えられるべきではありません。
