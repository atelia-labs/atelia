# Client UX

Atelia Mac と Atelia iOS は、単なる chat client ではありません。人間が Atelia を操作し、Secretary やほかのエージェントと協働するためのインターフェースです。project、thread、workspace、agent 作業、記憶、Hook、automation、Custom Extensions、差分、承認待ち、進行中タスク、policy を見渡し、必要なときに操作、承認、介入できます。

## 主な機能

- project / thread 単位で agent 作業を整理する
- 複数 agent の並列作業を自然に切り替えられる
- worktree や isolated workspace により repo 状態を汚さず試行できる
- branch、worktree、diff、commit、push、PR を自然に扱える
- 差分、判断ログ、承認待ち、実行結果を同じ画面で確認できる
- Secretary や外部レビューによる自動レビューで、変更のリスク、抜け、テスト不足、設計上の違和感を確認できる
- 複数ファイル、複数ターミナル、SSH devbox、アプリ内ブラウザを同じ仕事場で扱える
- アプリ内ターミナルと Git 操作が自然につながっている
- CLI / IDE / app / cloud の作業履歴と設定が分断されない
- plugin / skill / automation / extension を作成、導入、管理できる
- Hook、外部 event、policy、権限、監査ログを確認、管理できる
- review queue により、人間が必要なタイミングで戻ってこられる
- 音声による操作で、作業依頼、レビュー依頼、状況確認、承認、設定変更ができる

Git UX は Atelia の中核です。どの branch / worktree にどの agent の変更があり、どの差分がレビュー済みで、どれが承認待ちで、どれを取り込むべきか。人間と Secretary が同じ画面で理解できる状態にします。

GitHub 連携は重要な経路です。issue、pull request、review、discussion、release note への接続は、daemon と client の両方から扱います。

browser use / computer use は高度な機能であり、初期要件には含めません。
