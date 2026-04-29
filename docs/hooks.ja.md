# Hooks

Hook は、Atelia で起きる event だけでなく、GitHub Webhook のような外部 event にも反応し、automation、workflow、通知、記憶更新、extension による操作などを実行します。

ユーザーは、plugin や extension がなくても Hook を直接設定できるべきです。

ユーザーが作成した Hook と extension が作成した Hook は区別して表示します。特に extension が作成した Hook は、保護状態で確認できる必要があります。

Hook surface には次の情報を含めます。

- 作成元の user / extension / automation
- 発火条件、対象 event、event source
- 外部 event の検証方法
- 必要な権限と capability
- 実行される action
- 有効 / 無効 / blocked / needs approval などの状態
- 直近の発火履歴、変更内容、失敗理由
- Atelia が block した理由

Hook は、manifest と実際の挙動が一致している場合にのみ有効化できます。権限変更や発火条件の変更があった場合は、再承認を必要とします。
