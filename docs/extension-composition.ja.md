# Extension Composition

Atelia は personal agent harness です。extension は tool、memory、approval agent、外部サービス、review agent、delegated agent、workflow、presentation を追加できるため、仕事場はユーザーごとに違う姿になります。

その強さは conflict を生みます。Atelia は extension composition を implementation detail ではなく、first-class product surface として扱います。

## Composition Model

extension は surface に attach します。underlying contract は [Extensions](extensions.ja.md)、[Tool Output](tool-output.ja.md)、[Hooks](hooks.ja.md)、[Extension Security](extension-security.ja.md) を参照します。

- tool namespace
- tool output rendering
- hook / webhook routing
- workflow routing
- approval requests
- memory provider
- memory strategy
- notification routing
- presentation
- delegated agent pool
- service namespace
- resource namespace
- client extension surface
- external service account
- branch / repository ownership
- review queue

各 attachment は次を宣言します。

- scope: workspace, project, repository, branch, thread, agent, event type, resource namespace, resource
- mode: exclusive, pipeline, advisory, mirror, observer
- priority
- required capabilities
- produced state
- rollback behavior
- conflict policy

## Surface Modes

- `exclusive`: workspace の default memory strategy など、同時に1つだけ owner を持つ
- `pipeline`: tool-output renderer など、順序付き chain として動く
- `advisory`: approval agent や review agent など、複数 extension が decision を提案する
- `mirror`: notification など、同じ event を複数 consumer が受け取る
- `observer`: event や state を read-only で観測する

Secretary は各 surface の active composition を inspect できます。

## Resource Namespace

resource namespace は、extension が関与する対象を composition 上で識別するための scope です。家計、calendar、review queue のような domain を新しい一級概念として増やすのではなく、既存の surface / scope / mode に `resource_namespace` と `resources` を加えて表現します。

例:

```yaml
composition:
  attachments:
    - surface: tool_output.categorization
      scope:
        resource_namespace: household_finance
        resources:
          - transactions
          - budgets
          - bills
      mode: exclusive
      priority: 1
```

`exclusive` が `resource_namespace` と `resources` に対して働くことで、同じ対象を複数の extension が同時に所有しないようにできます。`pipeline`、`advisory`、`mirror`、`observer` は、同じ namespace への補助、通知、観測を表します。

resource namespace の粒度は extension が宣言します。ただし、粒度が粗すぎて他の extension を不必要に排除する場合、Atelia は conflict として表示し、scope を狭める resolution を提案できます。

## Conflict Detection

Atelia は enablement 前と runtime 中に conflict を検出します。

conflict の例:

- 2つの extension が同じ exclusive surface を claim する
- 2つの approval agent が同じ high-risk request を incompatible policy で approve できる
- 2つの delegated agent が同じ branch または task ownership を claim する
- 2つの extension が同じ resource namespace / resource で incompatible な exclusive ownership を claim する
- 2つの service provider が同じ service namespace を incompatible schema で claim する
- 2つの memory provider が同じ namespace を own しようとする
- 2つの memory strategy が同じ thread context を rewrite しようとする
- workflow extension が、別 workflow が own している event を reroute する
- presentation extension が required state を隠す
- integration extension が declared scope 外の protected external account に write する

conflict record は affected surface、extensions、scope、severity、available resolutions を持ちます。

## Resolution

resolution options:

- one extension を owner として選ぶ
- extensions を pipeline として順序付ける
- scope を狭める
- resource namespace または resources を分割する
- 特定 overlap だけ human approval を要求する
- 片方を advisory-only にする
- affected scope で片方を disable する
- 別 composition の workplace profile を作る
- review まで extension を quarantine する

Secretary は resolution に参加します。Atelia は resolution を提案できますが、persistent composition は workplace owner と Secretary が所有します。

## Approval Aggregation

Approval Agent は default では advisory です。提出された approval decision をどう compose するかは core policy が決めます。

aggregation modes:

- `single`: authorized approval が1つあれば通る
- `quorum`: N of M の authorized approval agent が approve する
- `human_final`: agent は recommendation を出し、human approval が決める
- `deny_veto`: authorized denial が1つでもあれば block する
- `risk_split`: low risk は agent approval、高 risk / critical risk は human escalation

各 approval surface は aggregation mode、risk ceiling、approver scope、expiration、human escalation path を宣言します。

## Workplace Profiles

workplace profile は named extension composition です。

examples:

- coding: Codex, Claude, GitHub, PR Resolve Agent, Approval Agent
- product: Linear, Slack, Claude, digest notifications, observational memory strategy
- autonomous development: Devin, Jules, CodeRabbit, GitHub, Approval Agent
- quiet mode: minimal notifications, strict approval, no delegated agents

profile は workspace または project ごとに切り替えられます。profile switching は composition diff を作り、capability surface が変わる場合は approval を要求します。

## Safe Mode

safe mode は community extension を disabled にして Atelia を起動します。official extension は manifest に明示された safe-mode profile、つまり read-only、unavailable、diagnostic のいずれかでのみ動けます。

safe mode で維持するもの:

- core protocol
- extension registry inspection
- extension disable / rollback
- basic file and shell harness surfaces
- composition diff viewer

壊れた extension composition が仕事場を閉じ込めないようにするため、safe mode を持ちます。

## Agent Colleagues

extension は colleague agent と subordinate agent を追加できます。

agent extension は次を宣言します。

- role
- provider
- model or external service
- task scope
- tool grants
- approval behavior
- branch / artifact ownership
- reporting format
- handoff behavior
- cancellation behavior

Secretary はこれらの agent に仕事を委譲し、Atelia contract を通じて evidence、artifact、status、review result を受け取ります。

## Approval Agents

Approval Agent は approval に特化した extension です。approval request を review し、人間が grant した scope の中で bounded decision を提出します。

宣言するもの:

- approvable capability scope
- risk tier ceiling
- evidence requirements
- escalation rules
- denial rules
- expiration behavior
- self-change restrictions

core approval boundary は、execution 前に decision を verify します。
