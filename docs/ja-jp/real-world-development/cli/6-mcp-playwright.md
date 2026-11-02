---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書く以外にも開発作業はあります。データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにすることも必要です。MCP server を使うと、Copilot CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を理解する。
- まだ利用できない場合は、Playwright MCP server を追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストや E2E テストも重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が作業中の Web サイトをユーザーと同じように操作できるようにし、期待どおりに動作する変更をより確実に作成します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部の情報にリアルタイムでアクセスし、ユーザーに代わって操作できます。

エージェントは、橋渡し役となる MCP server を通じてツールやリソースにアクセスします。各 MCP server は、エージェントが利用できるツールやリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP server があります。GitHub は、エコシステム内の発見や貢献を促進するために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI で設定されている場合は、セットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server の種類に **STDIO** を指定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で Copilot に機能を調べてもらう

先ほどは機能が期待どおりに動くことを手動で確認しました。今度は Copilot で利用できる Playwright MCP server を使って、同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、MCP server を指定しなくても Copilot が自動的に判断します。使うツールを明示しておくと、より安定した結果が得られ、余分なトークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. 結果を読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、レポートを提示します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使い、実際のブラウザーで機能を確認しました。次の作業を行いました。

- Model Context Protocol と Copilot CLI での利用方法を学んだ。
- まだ利用できない場合は、Playwright MCP server を追加した。
- エージェントにブラウザーを操作させ、フィルター機能を調べた。

次は、スキルとブラウザーツールを専門的な役割にまとめるため、[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP が注目される理由][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
