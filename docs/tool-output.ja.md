# Tool Output

Tool output は、AI エージェントにとっての AX surface です。Atelia は、tool の実行結果を人間向けログの転用として扱わず、AI が仕事を進めるための structured input として設計します。

## TOON First

Atelia の agent-facing tool output は、TOON を第一の形式として扱います。

TOON は Token-Oriented Object Notation です。LLM に構造化データを渡すための compact な表現であり、特に同じ key を繰り返す object array や tabular data で token 効率を高めやすい形式です。

JSON も必要です。Secretary は設定から TOON と JSON を切り替えられるべきです。さらに、一時的な format override と tool ごとの default format を持てるべきです。

これは Secretary と agent が自分の仕事場を育てるための AX customization です。

## Schema Design

Tool output の設計では、次を一つずつ問い直します。

- field の有無
- key name
- field order
- 省略
- 冗長性
- default value
- truncation
- artifact reference
- audit separation
- language
- token cost

raw observation、audit record、permission use、redaction、blocked reason は可視状態を保ちます。Extension が tool output を customize する場合も、truth は維持します。

## Language

ユーザーとの直接対話では、ユーザーの言語を尊重します。

agent-facing key、agent 間のやり取り、language-independent work では、英語を優先する選択肢を持ちます。これは token efficiency と task performance のためです。Secretary の preference、tool の性質、仕事の内容に応じて調整できるべきです。

## Operational Improvement

Atelia は、production operation log を AX 改善の入力として扱います。

通常の product が user behavior を分析して UX を改善するように、Atelia は Secretary と agent の tool use、format switching、repeated calls、unused fields、missing fields、token cost、AX Feedback を見て tool output を改善します。

目的は、Secretary と agent がよりよく働ける仕事場を育てることです。運用ログは AX 改善のために扱います。
