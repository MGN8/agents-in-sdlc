---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

これまでの演習では、ユーザー体験の改善と機能追加に関するいくつかのタスクを GitHub Copilot に依頼しました。コードにドキュメントを追加し、デザインチーム向けに関連ゲームを実装し、高コントラストとライトモードを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

## 作業をレビューする

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームメンバーがレビューを始める前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

### シナリオ

GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能をレビューします。必要に応じて Copilot に改善を依頼します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人の監視なしで作業するため、安全に作業できるよういくつかの制約があります。

- Copilot はリポジトリを読み取れますが、書き込みができるのはコード作業用のブランチだけです。
- Cloud agent は GitHub Actions 内に、作業用の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- MCP server などの[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加する最初のプルリクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP server などの外部リソースへのアクセスが既定で制限されているため、想定どおりの表示です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択し、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc コメントや、ほかのドキュメントが追加されたコードを確認します。実際の変更内容は異なる場合があります。TSDoc コメント、TypeScript の規約、ファイルヘッダーに注目してください。これらは演習 2 で確認したカスタム指示に基づいています。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっているか確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。単一の Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot とやり取りすることもできます。チームメンバーと同様に、PR やコードへのインラインコメントで Copilot にメンションします。Copilot はコメントを確認し、新しいセッションを開始して対応します。結果は確定的ではないため、必ず同じ文章を使う必要はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を説明するコメントを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例、または追加したい内容を使い、**@copilot** にメンションしてドキュメントの変更を依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新を行うため、新しいセッションが開始されます。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻るためのボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択し、変更を確認します。

満足するまで改善を依頼できます。問題がなければドラフトからレビュー可能な状態に変更し、`main` にマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっているか確認します。
7. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローが実行され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。完了には数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能が実装されました。cloud agent にコメントを追加すると、反復して改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc コメントを追加したりできます。後者を依頼した Issue は、カスタム指示を追加する前に割り当てたためです。PR の **Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - ARIA ラベル、キーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっているか確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。
10. **オプション:** Codespace でブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace のターミナルで Copilot が作成したブランチ名に置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーで高コントラストモードとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティエージェントが、アクセシビリティのベストプラクティスに従うよう Copilot をガイドしたことを確認します。アクセシビリティに関する懸念や改善点があれば、前の PR と同様に **@copilot** にメンションして更新を依頼できます。

## オプションの演習 — 委任を続ける

Cloud agent にはバックログから実際のタスクを任せ、自分は別の作業に取り組めます。この習慣を身につけるために、リポジトリに Issue を追加して Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次の作業を行いました。

- 後で cloud agent の出力を確認できるよう、このリポジトリのカスタム指示ファイルを確認した。
- Issue を Copilot cloud agent に割り当て、環境の構成、計画、非同期の実行を確認した。
- アクセシビリティ用のカスタムエージェントを作成して使い、高コントラストとライトモードのトグルを追加した。
- Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して作業中に指示を追加した。
- cloud agent のプルリクエストをレビューして改善し、**@copilot** に変更を依頼して、ワークフローを承認した。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかの環境での体験も確認する場合は、次のハーネスを参照してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の Agent モードと MCP を使います。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリで、次のような作業も試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野を担当するカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
