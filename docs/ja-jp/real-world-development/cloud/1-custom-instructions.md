---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に事前知識を与える場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に希望する作業方法を伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、この演習では更新前と更新後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue で非同期に作業するため、リアルタイムで影響を並べて示すのが難しいためです。後の演習で cloud agent が作成した PR をレビューするときに、指示ファイルの影響を確認します。

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

- `.github/copilot-instructions.md`: リポジトリに関する Copilot のすべてのリクエストに送られる単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、ほとんどのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種別に対する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> VS Code では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
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
> Copilot に指示ファイルを作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で方法が異なります。該当する場合は、各ハーネスのレッスンで説明します。
>
> テンプレートを探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示を確認する

プロジェクトには、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. 単体テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分け、参考例を指定できます。リンクのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、まだコード内ドキュメントの標準はありません。次の手順で追加します。

## チームの新しい標準を追加する

この演習で行う唯一の編集として、コード内のドキュメントに TSDoc コメントとファイル先頭のコメントを使うプロジェクト全体のルールを追加します。後の演習で Issue を割り当てると、cloud agent がこのルールを使います。

編集前に、作業用のブランチを作成します。Cloud agent は Issue のターゲットブランチから指示を読みますが、この変更もきれいにコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コーディング標準を追加して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目前後にある **Code formatting requirements** セクションを探します。現在はコーディング標準を説明していますが、コード内ドキュメントのルールがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル先頭のコメントと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、`.github/copilot-instructions.md` のようなリポジトリレベルのファイルに置くことも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` に置くこともできます。コードコメントのルールはプロジェクト全体の標準なので、リポジトリレベルのファイルに追加します。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、Issue のターゲットブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチを作成します。そのため、指示の変更を `main` にマージしておきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent がこの指示を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` にコミットしてもかまいません。実際のプロジェクトでの作業方法を紹介するためにブランチを使っています。

## まとめと次のステップ

プロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成する PR をレビューする際に、この標準が適用されていることを確認します。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
