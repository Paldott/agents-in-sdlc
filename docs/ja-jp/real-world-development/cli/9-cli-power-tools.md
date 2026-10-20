---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、並列作業、cloud agent への任意の委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、操作、内部情報の確認、追加設定に使うスラッシュコマンドが用意されています。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。このレッスンでは、ほかの便利なコマンドを確認します。

このレッスンでは、`/context` と `/compact` で会話のコンテキストを調べ、`/model` でモデルを確認し、`/share` でセッションをエクスポートまたは共有します。また、並列作業、worktree、cloud agent への委任に使う任意のコマンドも確認します。

## シナリオ

CLI の主要なワークフローが終わりました。コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて自動で会話を圧縮します。スラッシュコマンドを使って状況を確認し、手動で圧縮することもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリルートから起動します。
2. `/context` を入力します。
3. モデル、現在のトークン使用量、システム指示・ツール・メッセージ・空き領域へのコンテキストの割り当てを確認します。
4. `/compact` を入力して会話を圧縮します。
5. もう一度 `/context` を実行して結果を比較します。会話が短い場合、目立った変化がないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを自分で選ぶ場合は `/compact` を使います。無関係なタスクに切り替える場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選択する

モデルごとに強みが異なります。Copilot CLI で利用可能なモデルを確認し、選択できます。

1. `/model` を入力します。
2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、ローカルに Markdown を出力する `/share file` など、用途に合う保存先を選びます。
3. エクスポート内容を確認してから、他の人に送信したり公開したりします。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが公開を意図していないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業を拡張または委任する

主要なワークショップは完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクを複数のサブエージェントに割り当て、並行して実行します。
- `/worktree` は別のタスク用に分離された Git worktree を作成します。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で作業して PR を作成できるようにします。

これらのコマンドは worktree やリモート作業を作成するため、省略可能です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。

## まとめと次のステップ

スラッシュコマンドを使って Copilot CLI を設定し、セッションを共有し、内部の状態を確認しました。次は[ワークフローと今後のステップを振り返ります][next-lesson]。

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
