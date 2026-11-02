---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債が課題になっています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行します。作業が完了すると、人がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にする重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys では技術的負債の解消を目指しています。最初のサイトを作成した開発者は、ドキュメントをほとんど追加していません。まず、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの関連ゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は不要で、UX の受け入れテストに使える状態にします。この作業はブロッカーですが、優先度がより高い Issue がほかにあります。

このようなタスクは優先順位が下がりやすく、Copilot cloud agent に任せるのに適しています。開発者がほかの作業をしている間に非同期で処理し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人の開発者のようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot はドラフト PR を作成して進捗を記録し、環境を構成して作業を始めます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent にタスクを割り当てる際は、範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進めながら、学習、試行、調整を続けます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに完了している場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会に簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認します。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** を確認し、TypeScript の規約と TSDoc コメントの要件を読みます。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に指示を適用していることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成された PR に TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されます。後の演習で PR をレビューするときに確認します。

## Copilot cloud agent の開発環境を準備する

コードの作成では、必要なツールや依存関係を使えるように環境とセットアップスクリプトを準備することがあります。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルに[セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。演習の流れを支援するため、このワークフローはあらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションと初期データを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # この手順に必要な最小限の権限を設定します。Copilot は自身の操作に別のトークンを使用します。
    permissions:
      # セットアップ手順の一部としてリポジトリを clone し、依存関係をインストールする場合は contents: read が必要です。clone しない場合は、手順の完了後に Copilot が自動で実行します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用にローカル SQLite データベースをマイグレーションして初期化します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` というジョブが1つあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` が指定されているため、リポジトリの **Actions** タブから手動で実行できます。Copilot が実行するまで待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、作業効率を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成が得意なので、cloud agent に任せるのに適しています。作業はバックグラウンドで行われ、後の演習でレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て内容を示す画面](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。前に確認したワークフローを実行して環境を作り、プロジェクトを分析して、課題に取り組む方法を決めているためです。

13. TODO リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビューを示す画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解し、計画を考え、コードを作成する様子を確認できます。

作業には数分かかります。Copilot cloud agent は非同期に動作し、ほかの作業に集中できるのが大きな利点です。もう1つのタスクを cloud agent に割り当て、その間にアプリケーションの機能を実装しましょう。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent を使うと、作業を分けて、別のタスクに取り組みながらエージェントに作業を任せられます。デザインチーム向けに関連ゲームを表示する作業を Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。cloud agent は SWE と同じように作業するため、常に監視する必要はありません。完了後にレビューします。次は、カスタムエージェントを作成して使いましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使って、Issue を Copilot に割り当て、非同期に処理できることを学びました。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などを任せられます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にする重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業する間、カスタムエージェントを作成して使います。[Copilot cloud agent は MCP server も使用できます][cloud-agent-mcp]。前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
