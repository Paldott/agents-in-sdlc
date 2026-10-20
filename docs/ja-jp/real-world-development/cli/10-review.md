---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター Issue から始め、Plan モードで進め方を定義し、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、`quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を調べました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を確認しました。
- フィルター機能全体をレビューし、Agent Merge を有効にしました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` からブランチを作成し、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と評価がない状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルターの計画から PR 作成までは、同じ会話とブランチを使いました。手順を簡潔にするため、変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、ブラウザーでの手動確認、MCP を使った Copilot のブラウザー調査を行いました。`quality-checks` スキルは、単体テスト、lint、型チェックを新しい形式で実行・報告しました。QA エージェントは要件とテストカバレッジのレビューを結果に加えました。

追加テストで実際の不足を解消します。QA の実行で追加テストが不要という結論になる場合もあります。マージを許可する前に、コードと証拠を確認し、変更後には関連する証拠を更新します。

## ベストプラクティス

Copilot に提供するコンテキストとツールは、作業の結果に影響します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらを会話間で再利用し、チームの要件に合わせて調整します。指示は標準を定め、スキルは繰り返し行うタスクをまとめ、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。**Plan** は実装前の検討、**Interactive** は密に確認しながら進める変更、**Autopilot** は範囲が明確な作業に適しています。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じく、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に伝えることで、結果が変わります。

## さらに試す

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

ツールに習熟する最善の方法は、使い続けることです。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリで試してください。チームと知見を共有し、互いに学び、ドキュメントも確認します。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

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
