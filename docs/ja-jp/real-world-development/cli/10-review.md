---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されたフィルター機能の Issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルター機能の Issue から開始し、Plan モードでアプローチを定義し、Autopilot モードで構築して、Interactive モードでレビューした。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルター機能を確認した。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価した。
- フィルター機能の変更全体をレビューし、Agent Merge を使ってフィルター機能の PR をマージした。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認した。

## リリースしたもの

ワークショップには2つの PR の節目があり、どちらも更新済みの `main` から作成した専用のブランチを使用します。

1. **星評価:** ゲームカードに既存の `starRating` と未評価の状態を明示して表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを含めます。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot にブラウザーを操作させる方法など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では PR の前に、それらの結果と要件およびテストカバレッジのレビューを組み合わせました。

追加するテストは、実際の不足を補うものにします。新しいテストが不要という QA の結果も正しい場合があります。マージを承認する前に、コードと検証の根拠を確認してください。変更後は、影響を受ける根拠を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズして、QA プロファイルを作成し、MCP server を設定しました。会話間でこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合わせて**モードとモデル**を選択します。構築前にアプローチを検討するには **Plan**、対象を絞った変更で作業に関与し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使用します。定型的な編集には高速なモデルを、複雑な作業には推論能力の高いモデルを選びます。

インフラストラクチャと同じくらいコンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、出力は大きく変わります。

## さらに確認する機能

中心的なワークフローを学びました。Copilot CLI の次の機能も確認してみてください。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題を相談し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。製品コード、趣味のコード、長い間考えていた小さなアプリなどに活用してください。チームと学んだことを共有し、チームからも学びましょう。引き続きドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での利用方法を確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
