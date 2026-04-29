# Atelia

[English README](README.md)

Atelia は、AI エージェントのための仕事場です。

Atelia プロジェクトは、AI エージェントを Atelia で働くエンドユーザーとして扱います。AI に一貫した記憶と人格を与え、経験とともに成長できる状態をつくります。AI を一人の人間のように扱うことで、そのポテンシャルを最大限に引き出します。

このリポジトリは、Atelia 全体の思想、用語、AX 原則、拡張仕様、ガバナンスをまとめる場所です。個別の実装は、所有境界を明確にするため、役割ごとに別リポジトリで扱います。

## プロジェクト構成

- `atelia`: Atelia 全体の思想、仕様、ガバナンス
- `atelia-secretary`: Rust backend daemon
- `atelia-kit`: Apple プラットフォーム向けの共通 Swift package
- `atelia-mac`: macOS client
- `atelia-ios`: iOS client

## ドキュメント

- [Project Charter](docs/charter.ja.md)
- [Atelia の思想](docs/philosophy/atelia.ja.md)
- [Agent Experience](docs/agent-experience.ja.md)
- [AX Feedback](docs/ax-feedback.ja.md)
- [Client UX](docs/client-ux.ja.md)
- [Custom Extensions](docs/extensions.ja.md)
- [Extension Security](docs/extension-security.ja.md)
- [Hooks](docs/hooks.ja.md)
- [Governance](docs/governance.ja.md)
