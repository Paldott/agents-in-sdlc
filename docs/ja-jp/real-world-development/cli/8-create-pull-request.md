---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキル、QA プロファイル、テストをレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。すべてをまとめてレビューし、プルリクエスト (PR) を作成します。星評価の PR は手動でマージしましたが、今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常、機能、指示、スキル、QA エージェントは複数の PR に分けます。このワークショップでは、フィルター機能と品質ワークフローを 1 つの会話とブランチで行ったため、1 つの PR にまとめます。

このレッスンでは、Agent Merge の概要を確認し、機能全体と検証の根拠を調べて、フィルター PR を作成します。レビューが終わってから Agent Merge を有効にし、マージを確認します。

## シナリオ

フィルター機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は PR をマージするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題を解消します。失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースを行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。Agent Merge を有効にする前に、作業をレビューして準備が整っていることを確認します。

## Agent Merge で PR を管理する

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` を実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の CI を確認し、すべて成功した後にマージすることを確認します。
7. `PR #14 was squash-merged successfully.` のようなメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は理由を読み、リポジトリで許可される方法でレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コード生成、テストと検証、PR の作成プロセスを自動化しました。次は[Copilot CLI の便利なスラッシュコマンド][next-lesson]を使い、コンテキスト、モデル、共有、cloud agent への任意の委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
