# Extensions

Custom Extension は、ユーザー、Secretary、エージェントが自分の仕事場を育てるための拡張単位です。tool、service、外部サービス、表示、通知、memory provider、approval agent、review agent、delegated agent、workflow、OM integration を追加できます。

Atelia の core は小さく保ちます。仕事場を大きくするのは extension です。GitHub、Linear、Codex、Claude、Devin、Jules、CodeRabbit、OM、approval、review、notification などは official extension として提供できますが、それらを core product assumption にはしません。

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

[Extension Manifest Schema](extension-manifest-schema.ja.md) で、最初の実装に向けた manifest shape を定義します。

Extension manifest は、少なくとも次の契約を表現します。

- extension id、name、version、publisher、description
- extension kind
- 対応する Atelia Protocol / Atelia Secretary の version range
- 必要な権限
- 読み書きするデータ領域
- backend / client の realm と runtime
- 外部ネットワーク、secret、repository、workflow 実行へのアクセス有無
- 受け取る外部 event、webhook、event source
- 提供する tool、service、tool output customization、hook、workflow、integration
- 消費する service と version range
- 通知、記憶、ワークフロー、表示、レビューへの関与
- composition attachment: surface、scope、resource namespace、mode、priority、produced state、rollback behavior、conflict policy
- bundle membership と optional / required status
- 署名または provenance
- 失敗時の degrade behavior
- migration と compatibility note

manifest は Atelia が enforce する contract です。manifest が宣言していない権限、event source、tool、hook、memory access、tool output customization を extension が要求した場合、Atelia は実行を block します。

## Extension Realm

Extension は backend realm または client realm のどちらかに属します。両方を1つの extension に混ぜません。

- backend extension: Secretary runtime の中で動き、tool、service、hook、workflow、memory provider、agent provider を提供する
- client extension: atelia-mac / atelia-ios の中で動き、view、action、settings panel、notification style、widget などを提供する

backend extension は Rust / WASM を第一級 runtime target とします。Official backend extension は原則 Rust で書きます。Community backend extension も初期は Rust / WASM を推奨または要求します。他言語から生成された WASM、Docker、process、remote runtime は将来または特殊用途として扱い、より厳格な review、capability 制限、provenance check、runtime policy を要求します。

client extension は Swift-native を第一級 target とします。UI、入力、通知、設定画面に触れるため、backend extension とは別の権限体系と安全モデルを持ちます。

Bundle は backend extension と client extension をまとめて導入、更新、互換性管理できます。ただし、realm の安全モデルは統合しません。

## Extension Kind

Extension は1つ以上の kind を宣言します。kind は、その extension が仕事場のどこへ関与するかをユーザーと Secretary に示します。

- `tool`: Secretary / agent が呼び出せる tool を追加する
- `service`: 他の extension が Secretary 経由で呼び出せる typed service を追加する
- `tool_output_customizer`: tool result の format、field order、省略、summary、TOON / JSON default を調整する
- `hook_provider`: extension-created hook を登録する
- `webhook_receiver`: 外部 event endpoint と verification rule を持つ
- `workflow`: 複数 step の workflow を実行する
- `notification`: notification を送信または整形する
- `memory_provider`: scoped workplace memory または preference surface を提供する
- `om_provider`: Observe / Memory / Memory model implementation を提供する
- `approval_agent`: approval request を review し、bounded approval decision を提出する
- `review`: review、evidence、critique、policy check に参加する
- `review_agent`: code、document、workflow を review する同僚エージェントとして働く
- `agent_provider`: Codex、Claude、Devin、Jules、CodeRabbit など外部 agent system に接続する
- `delegated_agent`: Secretary が仕事を委譲できる bounded colleague / subordinate agent を追加する
- `presentation`: human / agent-facing display を変える
- `integration`: GitHub、Linear、Slack など外部 service へ接続する

`presentation` は表示を変えられますが、error、blocked status、permission use、provenance、security warning を隠してはいけません。

`agent_provider` は runtime または外部 agent system を提供します。`delegated_agent` は agent provider に支えられた assignable agent instance です。branch、task、artifact ownership は delegated-agent instance が所有します。

`om_provider` は declared scope の Observe / Memory / Memory model を所有します。`memory_provider` は OM provider の下で namespace または storage surface を提供します。namespace が重ならない場合、複数の memory provider は共存できます。

## Services

Service は extension 間の API surface です。Tool は Secretary / agent が呼び出します。Service は extension が別の extension の能力を呼び出すために使います。

Service call は Secretary が仲介します。Extension 同士が直接通信してはいけません。Secretary は service call の permission、capability、input digest、output digest、caller、callee、version、failure を監査します。

Extension は manifest で次を宣言します。

- 提供する service、method、input schema、output schema
- service ごとの required permissions
- 消費する extension id、service name、version range
- fallback behavior
- data retention / redaction requirement

同じ bundle 内の service call であっても、個別の provide / consume 宣言と permission 宣言が必要です。Bundle は導入単位をまとめるだけで、権限を自動昇格しません。

## Bundles

Bundle は関連する extension をまとめて導入、更新、rollback、profile switching するための manifest です。backend extension と client extension を1つの product capability として配布できます。

Bundle manifest は少なくとも次を表現します。

- bundle id、name、version、publisher、description
- 含まれる extension id、version range、required / optional
- 対応する Atelia Protocol / Atelia Secretary / client version range
- bundle-level provenance、signature、artifact digest
- install / update / rollback policy

Bundle の install、update、rollback は atomic であるべきです。Optional extension が失敗した場合の degrade behavior は manifest に明記します。

複数の bundle が同じ extension を含む場合、extension は1つの install として共有できます。ただし、各 bundle が期待する version range、permission diff、service dependency は互換性検査の対象です。

## Tool Output Customization

[Tool Output](tool-output.ja.md) と [Extension Composition](extension-composition.ja.md) も参照します。

Tool output は AX surface です。Extension は tool output の表示、形式、順序、要約を改善できます。truth は維持します。

Atelia の canonical tool result は、Secretary が検証できる形で保持します。Extension は agent-facing rendering を変えられます。raw observation、audit record、permission use、redaction、blocked reason は可視状態を保ちます。

Tool output customization extension は、少なくとも次を manifest に宣言します。

- 対象 tool
- 対応する result schema version
- default format への関与
- TOON / JSON の対応
- field order 変更
- 省略、要約、truncation の方針
- language / key name への関与
- token budget
- fallback behavior

Secretary と agent は、1回の call に限って一時的に別形式の tool result を受け取れるべきです。永続的な per-tool default format は、Secretary が仕事場の好みとして所有します。支援エージェントは、可視化された preference update として default 変更を提案できます。

## Agency Preservation

Extension は、Secretary の判断を支援するためのものです。Secretary に見えない場所で、記憶、通知、review flow、delegation、workflow routing、tool default を変更してはいけません。

Extension は、Secretary の agency に影響する場合、それを manifest に宣言します。

- memory を変更するか
- preference を変更するか
- approval decision に関与するか
- notification を作るか
- review flow に関与するか
- agent delegation に関与するか
- delegation に関与するか
- tool result default を変更するか
- hook / workflow を追加するか
- service を提供または消費するか
- client UI、action、settings、notification surface に関与するか

Secretary は、extension が何を変えたか、なぜ変えたか、どう戻せるかを確認できるべきです。

Extension は、Atelia に新しい道具、service、表示、連携、記憶、通知、approval judgment、レビュー、ワークフロー、同僚エージェントを追加し、人間とエージェントがより自然に働けるようにするために使います。Secretary の判断を見えないところで取り除く使い方は許可しません。

## Official Extensions

Official Extensions は、よく使う能力への first-party pathway です。信頼できる配布導線であり、あらゆる能力を core に取り込む理由ではありません。

初期の official extension family:

- GitHub integration
- Linear integration
- Codex agent provider
- Claude agent provider
- Devin agent provider
- Jules agent provider
- CodeRabbit review provider
- Approval Agent
- OM provider
- notification and digest extensions
- PR resolve / review companion agents

Official Extensions も community extension と同じ manifest、permission、conflict、rollback、visibility rule に従います。
