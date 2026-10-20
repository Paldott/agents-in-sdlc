---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。非同期かつ自律的に動作するエージェントの作業も、監視が重要です。

GitHub の[エージェントページ][agents-page]では、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに指示を追加できます。

このレッスンでは、エージェントページで cloud agent のタスクを監視し、進行中のセッションに機能追加を依頼します。

## シナリオ

ハイコントラストモードの実装中に、チームはライトモードも追加することにしました。エージェントに作業を追加で依頼し、両方のモードを実装させます。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインに表示されるタスクを確認します。ドキュメントの更新、ゲーム更新用 API の生成、サイトへのハイコントラストモード追加などが表示されます。
3. 実行中のタスクを選び、Copilot が行った作業を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスクの実行などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインで、PR と変更されたファイルを確認します。

## cloud agent に指示を追加する

実行中のタスクを確認したら、ハイコントラストモードに加えてライトモードを依頼します。

1. ハイコントラストモードを追加するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで数分待ちます。Copilot のメッセージから、作業を開始したことが分かります。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot が作業中のエージェントタスクで、セッションへの追加指示ダイアログを表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して送信します。
5. Copilot が依頼を受け取り、現在の作業が終わった後に新しい要件を取り込むことを確認します。

## Copilot の作業を待つ

Copilot は現在の手順を完了してから、新しい依頼を作業に反映します。非同期処理の間に、ここまで学んだことを振り返ってください。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent タスクを監視して案内する中央ページである Copilot agents ページを使いました。実行中・完了済みのタスクを追跡し、実行中の作業をレビューして、追加機能を依頼するようセッションを方向付けしました。

次は[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
