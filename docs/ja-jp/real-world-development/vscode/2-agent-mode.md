---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さな機能の追加でも、通常は複数のファイルを更新し、テストを実行するなどの作業が必要です。開発者は、関連ファイルを見つけて変更し、テストを実行し、デバッグし、更新し忘れたファイルを見つけ、さらに修正するという流れを繰り返します。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業できるよう設計されています。開発者と同様に、プロジェクトの構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは、Agent Mode を使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、Agent Mode と cloud agent は異なる体験を提供します。Agent Mode は IDE 内で動作するため、すばやいフィードバックとやり取りが可能です。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを見つけて、複数ファイルを連携して変更する。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの数が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。ここでは Copilot Agent Mode を使い、残りのデータ層、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため Tailspin Toys の Web サイトを確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページの frontmatter から Drizzle ORM を使ってローカル SQLite データベースを直接検索し、別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、Astro の開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後に `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、機能を追加します。まずバックログを確認します。テンプレートからプロジェクトを作成したときに、Issue のバックログが自動的に用意されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は自律的にプロジェクトを計画、実装し、ターミナルコマンドの実行やツールの呼び出しを行います。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨する意図はありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。体験をできるだけ一貫させることが目的ですが、LLM の動作は確率的なので、提案が記載内容と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは実行できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える順に項目を並べていることを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、プロジェクトのコンテキストを与える指示ファイルを確認します。この演習では、Web サイトへの機能追加方法を説明する [UI][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するためのガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト可能性、アクセシビリティの原則、ファイル種別ごとの指示へのリンク (Astro、Tailwind CSS、Drizzle など) が含まれています。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクトのファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用可能な小さなファイルに分け、参考例を指定できます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の3種類の更新が必要です。

- データ層 (`src/lib/`) の絞り込みロジックを追加または改善する。
- 絞り込みの動作を確認するテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらを依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連ファイルを見つけて更新し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、数分かかる場合があります。その間にエディターで **Keep** と **Undo** が表示されることがあります。Copilot が作業を終えると、すべての変更に対して選択肢が表示されるため、作業中に選ぶ必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中にテストが失敗したり、処理が一時停止したりする場合があります。Copilot は、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを確認しました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを見つけて連携して変更する。
- コードベースにマージする前に、生成された変更とテストをレビューする。

次は、[Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加演習 — ページネーションを実装する

ゲームが増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot に実装を依頼してください。次の点を検討します。

- 既存の指示ファイルを使うなど、ベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定にするかを考える。
- 希望する実装に必要なガイダンスをプロンプトで伝える。
- GitHub Copilot に変更とコンテキストを追加で伝えながら、何度か反復する場合があります。Copilot を使う通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モードの違いと使い分け][choose-mode]
- [VS Code の Agent モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
