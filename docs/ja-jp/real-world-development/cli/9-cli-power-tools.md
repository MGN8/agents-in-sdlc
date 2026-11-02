---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが用意されています。コマンドから、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、必要に応じて [Copilot cloud agent][about-cloud-agent] に作業を委任するなど、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模の大きいタスクや複雑なタスクでは、モデルのコンテキストウィンドウが上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使えば、自分で確認したり圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していなければリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。`/compact` を使うと、圧縮するタイミングを自分で選べます。無関係なタスクに切り替えるために新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと利用状況を確認します。
3. 現在のモデルを使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown への書き出しには `/share file` など、用途に合った出力先を選択します。
3. 誰かに送信したり公開したりする前に、エクスポートした内容を確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には、大規模なタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、非同期に作業してプルリクエストを作成する場合もある Copilot cloud agent にタスクを送信します。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意です。試す場合は、まず新しい会話で範囲を明確にしたタスクを開始し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、ツールを構成し、セッションを共有し、内部の状況を確認できます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` で、Copilot がコンテキストを管理する方法を確認した。
- `/model` で、利用可能なモデルを調べた。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。最後に[学んだ内容][next-lesson]を振り返り、これからの学習に向けた次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
