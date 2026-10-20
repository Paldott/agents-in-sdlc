---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、コードへのドキュメント追加、関連ゲーム機能、ハイコントラストとライトモードを含むアクセシビリティ機能を Copilot に依頼しました。コードの変更を確認し、必要に応じてフィードバックを伝えます。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。コードをレビューし、通常の DevOps プロセスに沿って進めます。チームのレビューを有効にする前に、ドキュメント、関連ゲーム、アクセシビリティ機能の PR を確認します。

## GitHub Copilot cloud agent のセキュリティ

cloud agent は非同期かつ自律的にタスクを実行するため、安全性を確保する制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用ブランチに限定されます。
- GitHub Actions 内にタスクごとの一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した、コードへのドキュメント追加 PR を github.com で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーへの接続を含め、外部リソースへのアクセスは既定で制限されているため、これは想定された動作です。[Copilot cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. github.com のリポジトリを開きます。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルの PR を開きます。
4. PR に **[WIP]** が付いている場合は、Copilot の作業が完了するまで待ちます。
5. **Files changed** タブを選択して変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

6. TSDoc コメントなどのドキュメントを確認します。変更内容は異なります。演習 2 で確認したカスタム指示による TSDoc、TypeScript 規約、ファイル先頭コメントも確認します。
7. **Conversation** タブに戻り、下にスクロールします。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローの実行を確認します。単一の Astro アプリに対するプロジェクトのチェックが成功するまで待ちます。

## GitHub Copilot に変更を依頼する

PR のコメントで **@copilot** をメンションすると、追加の変更を依頼できます。たとえば、コードファイルへのコメントヘッダー追加、TypeScript と Astro ファイルへの TSDoc コメント追加、Astro アプリ構成を説明する README の作成などを依頼できます。

1. **@copilot** をメンションしたコメントで、生成されたドキュメントへの変更を依頼します。
2. **View Session** を選択して、Copilot が更新する様子を確認します。
3. **Back to pull request** を選択して PR に戻ります。
4. Copilot の作業が完了したら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブで変更をレビューします。納得できるまで反復し、準備が整ったら PR をレビュー可能にして `main` にマージします。

## 関連ゲーム機能をレビューする

1. リポジトリの **Pull Requests** タブを開きます。
2. **Show related games on the game details page** などのタイトルの PR を開きます。
3. **Files changed** タブでコードをレビューします。
4. **Conversation** タブに戻り、承認待ちワークフローがあれば **Approve and run workflows** を選択します。
5. チェック欄のワークフローが完了し、プロジェクトのチェックが成功するまで待ちます。
6. 省略可能: Codespace のターミナルで、Copilot が作成したブランチに切り替えて機能を手動でテストします。`<branch-name>` は実際のブランチ名 (例: **copilot/fix-8**) に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要に応じて PR の **Conversation** タブにコメントし、Copilot に変更を依頼します。この Issue はカスタム指示を更新する前に割り当てたため、コメントヘッダーや TSDoc コメントの追加も依頼できます。

## アクセシビリティ機能をレビューする

この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で追加を依頼したライトモードの両方が含まれます。

1. リポジトリの **Pull Requests** タブで、**Add high contrast mode to website** などのタイトルの PR を開きます。
2. **[WIP]** が付いている場合は、Copilot の作業が完了するまで待ちます。
3. **Files changed** タブでコードを確認します。特にモード切り替え UI、設定を保存する local storage、各モードの CSS、ARIA 属性やキーボード操作、モード切り替えの JavaScript/TypeScript を確認します。
4. **Conversation** タブに戻り、承認待ちワークフローがあれば **Approve and run workflows** を選択します。
5. プロジェクトのチェックが成功するまで待ちます。
6. 省略可能: Codespace で Copilot が作成したブランチに切り替えます。`<branch-name>` を実際のブランチ名に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

7. アプリを起動し、ブラウザーで両方のモードを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティエージェントがガイドラインに沿った実装を支援したことを確認します。問題や改善点があれば、PR コメントで **@copilot** に変更を依頼します。

## 省略可能な演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せ、ほかの作業に取り組む習慣を身に付けるため、リポジトリに Issue を追加して Copilot に割り当てます。たとえば、ゲーム詳細ページの支援者向けフォーム、ゲーム一覧のページネーション、データアクセスヘルパーの入力検証とエラー処理を依頼できます。

## まとめ

Cloud agent ハーネスを完了しました。指示ファイルを確認し、Issue を割り当て、非同期の環境構築と作業を監視しました。アクセシビリティ用カスタムエージェントを使い、エージェントページからセッションを案内し、PR をレビューして **@copilot** に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスはこれで完了です。ほかのハーネスでは、関連するシナリオを異なる環境で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェントモードと MCP 連携を使います。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA エージェント、Playwright MCP、Agent Merge、`/delegate` による cloud agent への任意の委任を使って、ターミナルから 2 つの変更を届けます。

自分のリポジトリでは、リファクタリングやテストカバレッジの Issue を cloud agent に割り当てたり、別分野のカスタムエージェントを作成したり、別のスタック用の `copilot-setup-steps.yml` を設定したりできます。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
