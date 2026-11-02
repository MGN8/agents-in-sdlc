---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP server** を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP server が Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP server をワークスペースに追加する。
- Copilot にブラウザーでフィルター機能を手動テストするよう依頼する。
- ブランチを公開して、フィルター機能のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできるとさらに便利になります。Copilot は Model Context Protocol (MCP) を使って、外部ツールやサービスと通信します。MCP は、エージェントが外部のツールやサービスと連携するための標準的な方法です。

![エージェントモードがコンテキスト、LLM、ツールと連携する仕組みを示す図。MCP server と VS Code 拡張機能が提供するツールを含む。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。最新情報 (リソース) にアクセスしたり、ユーザーに代わって操作 (ツール) を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスとの橋渡しをする MCP server を通じてアクセスします。MCP server は、既存の API や npm パッケージなどのローカルツールを含む、外部のツールやサービスとの通信を管理します。各 MCP server は、AI エージェントが利用できるツールやリソースを提供します。

すでに利用できる MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。リポジトリの作成や更新、Issue やプルリクエストの管理などをエージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を利用できます。Web ページの表示、フォームへの入力、ボタンの選択などをエージェントに依頼できます。

ほかにも多くの MCP server があり、GitHub はエコシステム内の発見や貢献を促進する [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP server はプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使用し、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP server を構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` server が設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP server][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、GitHub のツールを初めて使うときに認証を求めます。

## Playwright MCP server を追加する

次に2つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot がブラウザーを操作できます。これでフィルター機能をテストできます。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` server と異なり、`playwright` はローカル server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示せずにブラウザーを動かす設定です。デスクトップ環境がない Codespace で必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使うため、通常はブラウザーもインストール済みです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。Copilot が初めて server のツールを必要とするときに server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されたら選択し、server を起動します。これで構成が正しいことを確認できます。
2. VS Code から server の信頼を求められた場合は、構成を確認し、信頼するよう選択して起動します。

手動で起動する必要はありません。スキップすると、次の手順で Copilot が初めてツールを使うときに VS Code が起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動やフィルターの適用を行わせて、結果を確認してもらえます。会話を離れずに機能を手早く確認できます。

Playwright MCP server はスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を参照します。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を使ってエージェントが判断します。支援技術と同じ情報を使うため、簡単な機能確認をアクセシビリティの確認にも活用できます。

server を接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を使ってブラウザーを起動し、各手順を確認して結果を報告します。結果を Issue の受け入れ条件と照らし合わせます。問題があれば、プルリクエストを開く前に質問するか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリを実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP server を初めて使うと、ブラウザーがダウンロードされる場合があります。不足が報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューできるよう PR を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンドライン インターフェイス (CLI) などで作成できます。ここでは GitHub Copilot に作成を依頼します。関連する Issue を見つけ、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを Copilot に送信して PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際は、必要に応じて **Continue** を選択して許可します。GitHub ツールを初めて使う場合は GitHub への認証を求められることがあります。画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol と、MCP server が Copilot にツールを追加する方法を学んだ。
- Playwright MCP server をワークスペースに追加した。
- Playwright MCP server でブラウザーを操作し、リリース前にフィルター機能を手動テストした。
- ブランチを公開して、フィルター機能の PR を作成した。

MCP server を使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの対象を絞ったタスクを効率化するため、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP が注目される理由][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
