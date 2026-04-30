# Atelia

[English README](README.md)

Atelia は、あなたによる、あなたのための、あなただけのエージェント・ハーネスです。

Atelia は、Secretary のための personal で extensible な multi-agent workplace です。Secretary は、小さな core と大きな extension surface を通して、tool、approval agent、memory provider、外部サービス、同僚エージェントを orchestrate します。

このリポジトリは、Atelia のプロダクトコンセプト、用語、AX 原則、拡張仕様、ガバナンスをまとめる場所です。個別の実装は、所有境界を明確にするため、役割ごとに別リポジトリで扱います。

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
- [Tool Output](docs/tool-output.ja.md)
- [Client UX](docs/client-ux.ja.md)
- [Extensions](docs/extensions.ja.md)
- [Extension Composition](docs/extension-composition.ja.md)
- [Extension Security](docs/extension-security.ja.md)
- [Hooks](docs/hooks.ja.md)
- [Versioning and Releases](docs/versioning.ja.md)
- [Governance](docs/governance.ja.md)
