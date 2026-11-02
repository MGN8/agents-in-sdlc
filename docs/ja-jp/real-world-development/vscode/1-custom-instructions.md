---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に事前知識を与える場合は、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成に関するガイダンスを Copilot に与えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する。
- 同じプロンプトを再実行し、指示の変更が生成コードに与える影響を確認する。
- 指示の変更とフィルター機能のコードを `main` にコミットし、次の演習で更新済みのガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習の目的は、指示の更新による傾向の変化を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys では、次の開発ガイドラインを定めています。

- データ層には必ず単体テストを追加する。
- UI はダークモードで、現代的な見た目にする。
- TSDoc のコメントを使ってコードにドキュメントを追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加する。

指示ファイルを使い、これらのプラクティスに沿って Copilot が作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に渡せます。希望するコーディング規約、ライブラリ、コードコメントの形式などを指定して、より適切な提案を得られます。リポジトリ全体または特定のファイル種別向けに指示を作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種別に対する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルの管理に関するベストプラクティス

この演習では指示ファイルの作成方法をすべて説明しません。サンプルプロジェクトを確認し、次のような代表的な方法を学びます。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用する標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに対する指示を記載します。
- 自然な言葉を使い、ガイダンスをわかりやすくします。望ましい例と避けるべき例を示します。

指示ファイルの使い方に唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルを確認できます。
>
> Copilot に指示ファイルを作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で方法が異なります。この演習では、該当する場合に説明します。
>
> テンプレートやひな形を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示を確認する

プロジェクトには、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. 単体テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分け、参考例を指定できます。リンクのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、まだコード内ドキュメントの標準はありません。次の手順で追加します。

## 変更用のブランチを作成する

変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. 新しいターミナルを開くには、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. 次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示の更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストなどを更新します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約に基づいています。
7. 生成されたコードに TSDoc コメントとファイル先頭のコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後、一貫性が向上することを確認するのが目的です。

## リポジトリ全体の標準を追加する

前の手順で確認したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コードの提案を改善するため、コーディング標準にドキュメントのルールを追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目前後にある **Code formatting requirements** セクションを探します。現在はコーディング標準を説明していますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル先頭のコメントと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、`.github/copilot-instructions.md` にはリポジトリ全体の指示を、`*.instructions.md` には言語、ファイル種別、タスク固有の指示を記載します。ドキュメントコメントのルールはプロジェクト全体に適用するため、リポジトリレベルのファイルに追加します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. Copilot が適切なファイルに注目するよう、`src/lib/publishers.ts` を再度選択します。
3. 同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディングプラットフォーム向けパブリッシャーデータアクセスヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含む、すべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns パブリッシャーオブジェクトの配列に解決する Promise。
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使う機能に適用しました。

## 指示と変更をコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同じようにソース管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示とヘルパーの変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の更新 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. リポジトリにブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、Copilot に GitHub ツールを使わせ、バックログのドキュメント標準に関する Issue を見つけて関連付けた PR を作成し、マージします。

> [!NOTE]
> GitHub 上の操作を Copilot に依頼する最初の演習です。プロジェクトテンプレートでは GitHub ツールが接続されているため、追加設定はありません。Copilot が初めて使うときに VS Code から GitHub へのサインインを求められた場合は、画面の指示に従ってください。後の演習では、この接続に使われる Model Context Protocol について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** を選択します。
3. 現在のブランチから PR を作成し、指示の更新とパブリッシャーヘルパーの土台を説明して、関連 Issue を閉じるよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する間、処理を確認します。
5. Copilot による GitHub の操作を**許可**するよう求められた場合は、コマンドを確認してから適切に **Allow** を選択します。
6. PR が作成されたら、Copilot にマージを依頼し、ローカルのブランチを `main` に戻すよう求めます。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められた場合は、コマンドを確認してから適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認しました。VS Code の Copilot Chat で次の作業を行いました。

- コード生成のプロンプトを送り、既存の指示で Copilot が生成するものを確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、指示の追加が提案コードに反映されることを確認した。
- 次の演習で使えるように、指示とパブリッシャーヘルパーの土台を `main` にコミットした。

次は、[エージェントモード][next-lesson]を使い、Copilot にコードベース全体の機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
