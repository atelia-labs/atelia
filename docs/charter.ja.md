# プロジェクト憲章

## ミッション

Atelia は、あなたによる、あなたのための、あなただけのエージェント・ハーネスです。

Atelia は、Secretary のための personal な multi-agent workplace です。小さな core が harness boundary を提供し、extension が tool、memory provider、approval agent、外部サービス、workflow、presentation、同僚エージェントを追加します。

AI エージェントを serious worker として扱うという広い思想は Atelia Labs が引き受けます。Atelia は、その思想を、ユーザーごとに育てられる agent harness としてプロダクト化します。

## 約束

1. 人間の owner は、自分専用の Atelia workplace を持つ
2. Atelia の第一の品質は、AI エージェントにとっての体験、すなわち AX（Agent Experience）である
3. Atelia は、単なる技術的最小限ではなく、MDP（Minimum Desirable / Delightful Product）として育てる
4. ユーザーと Secretary は、extension を通じて自分の仕事場を育てられるべきである
5. 仕事場改善では、人間の owner と harness の中で働く agent の声をどちらも重視する
6. Secretary は tool、workflow、同僚エージェントを orchestrate する
7. extension は仕事場を作り替えられるほど強くできる。capability boundary、approval、rollback、visibility により agency を保つ

## リポジトリ境界

このリポジトリは Atelia 全体の方向性と仕様を扱います。

具体的な実装は、それぞれ別リポジトリで扱います。Secretary は Rust daemon として、Mac/iOS client はネイティブアプリとして、Atelia Kit は共通 Swift package として育てます。
