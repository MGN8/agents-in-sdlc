---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

この2つの演習では、ユーザー体験の改善と機能追加に関する3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールには、GitHub.com の[エージェントページ][agents-page]があります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業の内容を調べ、進行中のセッションに指示を追加することもできます。

このレッスンでは、次の作業を行います。

- エージェントページを開き、cloud agent のタスクを監視する。
- 進行中のセッションに指示を追加する。

## シナリオ

エージェントに高コントラストモードを作成するよう依頼した後、チームはライトモードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが適切です。エージェントに指示し、高コントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。コードのチェックアウト、作業環境の作成、MCP server の設定、タスクを完了するための各手順などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の進捗や作業方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更ファイルを見て、追加で監視できます。

## Cloud agent に指示を追加する

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. 高コントラストモードのセッションを選択します。エージェントの表示名や作業状態によって、タイトルは異なる場合があります。

   ![mission control に表示されるアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。開始すると、Copilot からメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページの cloud agent タスクで、作業中のセッションに指示を追加するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れることを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。プロンプトを送信した時点で作業していた手順が終わると、追加の要望も処理します。

この作業にも数分かかります。その間に、これまで学んだ内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、Copilot cloud agent のタスクを監視し、方向付ける方法を学びました。mission control から実行中と完了済みのタスクを確認し、進行中のタスクに指示を追加できます。

このレッスンでは、次のことを確認しました。

- mission control とエージェントページで cloud agent のタスクを監視した。
- 進行中のセッションに指示を追加して機能を拡張した。

Copilot がアクセシビリティ機能を実装する間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
