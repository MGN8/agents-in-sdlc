---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue からフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って作業できるため、適切なものを適切な方法で構築できます。このレッスンから数回にわたり、Issue から新機能を実装し、コードが有効で期待どおりに動作することを確かめ、最終的にプロジェクトへマージする、一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心となる概念に集中できるよう手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から新しい Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズして、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えており、訪問者がゲームをカテゴリーとパブリッシャーで絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、Autopilot による範囲を限定した実装を承認します。

## 背景

AI コーディングエージェントを開発フローに取り入れても、基本は変わりません。むしろ、さらに重要になります。多くの開発者は、次のようなフローに従います。

1. 作業内容を説明した Issue から始める。
2. 実装する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行して検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 正確な手順はチームや Organization によって異なりますが、多くの場合は上記の流れをもとにしています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、人が作成したコードと同じレビューのプロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、続行する前にユーザーの入力を待ちます。
- **Plan**: エージェントが計画を作成します。計画中はプロジェクトファイルを編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

Plan モードで開始し、計画をレビューしてから、Autopilot で実装します。

## Issue から開始する

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエストをコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、機能の実装やタスクの実行方法を Copilot に任せる前にアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に基づいて回答します。正解は1つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. スクロールして計画を確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使うと、Copilot が作業を反復して完了を目指します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様のオプションを選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って作業を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェックを完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードも、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容に照らし合わせます。
4. コードレビュー後、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャー、およびその組み合わせで絞り込みを試します。
4. リセットと一致結果がない場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、文書化されたプラクティスにコードを従わせるため、[カスタム指示を使用します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
