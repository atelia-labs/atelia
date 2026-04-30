# Versioning and Releases

Atelia は、実装リポジトリごとに独立した version stream を持ちます。

## Version Streams

- `atelia`: プロジェクト全体の docs と仕様。release は milestone 単位で扱い、必要なら dated tag を使います。
- `atelia-secretary`: Rust daemon。SemVer を使います。
- `atelia-kit`: 共通 Swift package。SemVer を使います。
- `atelia-mac`: macOS native client。installable app build ができた段階で SemVer を使います。
- `atelia-ios`: iOS native client。installable app build ができた段階で SemVer を使います。

## Pre-1.0 Rule

`1.0.0` までは、minor version で breaking change を許容します。ただし release note で明示します。

patch version は、同じ minor version 内で互換性を保つべきです。

## Public Contracts

次の surface は、実装 repository または extension author が参照した時点で public contract として扱います。

- extension manifest schema
- permission taxonomy と risk tier
- hook / webhook schema
- tool-output result schema と status code
- Atelia Protocol API と generated client artifact
- registry、provenance、blocklist metadata
- extension / client が宣言する compatibility range

## Change Classification

| Change | Release handling |
| --- | --- |
| optional manifest field、optional hook field、optional tool-result field、既存 extension が要求しない新 permission の追加 | compatible change |
| documented fallback behavior を持つ enum value の追加 | compatible change |
| field の rename / removal、field meaning の変更、status semantics の変更、fallback behavior の変更 | release note の breaking note、pre-1.0 minor bump |
| permission requirement の強化、default tool-output format の変更、agent が依存する field order の変更、hook verification behavior の変更 | breaking note、必要に応じて migration note |
| manifest schema version、permission model、persisted extension state、registry metadata、tool-output major schema の変更 | migration note と compatibility-range update |

## Tags

tag は各 repository が所有します。

- 実装 repository は `vMAJOR.MINOR.PATCH` を使う
- project-wide docs は、governance / specification snapshot を示す場合に `vMAJOR.MINOR.PATCH` または dated tag を使える

## Release Notes

release note には次を含めます。

- summary
- breaking changes
- migration notes
- security notes
- contributors
- 重要な issue / pull request への link

## Initial Versions

初期公開開発では、usable release artifact または documented API surface ができた repository から `0.1.0` を開始します。
