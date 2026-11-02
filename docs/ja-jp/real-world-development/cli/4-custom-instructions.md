---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。作業方法に決まりがある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント作成の標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認する。
- コーディング標準が守られるように指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys では、次の開発ガイドラインを定めています。

- コメントでは、コードの言い換えではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付ける。`db` 引数を注入できる場合は、その旨も記載する。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更に合わせてコメントを更新する。
- 既存の書式と lint に関するガイダンスを維持する。

指示ファイルを使い、これらのプラクティスに沿って Copilot が作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に渡せます。プロジェクト全体の指示と、特定のファイルやタスク向けの指示を作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関する Copilot のすべてのリクエストに送られる単一の指示ファイルです。多くのリクエストに共通する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のプログラミング言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式の指示ファイルとそのサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示を確認する

スターター プロジェクトには、すぐに利用できる指示ファイルが含まれています。変更を加える前に、どのような指示があるかを確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分割し、参考例を指定できます。

## チームのガイダンスに合わせて指示を更新する

用意された指示は有用ですが、まだ不足があります。`.github/copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc コメントが付くようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35行目前後にあります。
2. 次の行を、このセクションの最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、機能のコンテキストを保ったまま Copilot CLI を終了して、フィルター機能の会話を再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI が会話を自動的に保存します。
2. `game-filters-cli` ブランチにいることを確認し、リポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認した。
- コーディング標準が守られるように指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、lint とテストを一貫して実行できるよう、再利用可能な [quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
