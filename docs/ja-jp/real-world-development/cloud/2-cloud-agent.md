---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債の解消、古いコードの更新、バックログに残った機能の実装など、開発チームには後回しになりがちな作業があります。GitHub Copilot cloud agent はコードを更新したり機能を追加したりするタスクを自律的に実行し、完了すると人間がレビューできるドラフト PR を作成します。時間のかかる作業を任せ、開発者がより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って、コード生成環境のカスタマイズ、安全な操作、範囲の明確な Issue の重要性、Copilot への Issue の割り当てを確認します。

## シナリオ

Tailspin Toys では、アプリケーションのエクスポート関数に TSDoc コメントがありません。まず、すべてのエクスポート関数にコメントを追加します。また、デザインチームはゲーム詳細ページに同じカテゴリの関連ゲームを表示し、UX の受け入れテストを行いたいと考えています。優先度は高くありませんが、どちらも非同期で処理できる cloud agent に適した作業です。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行します。[GitHub Issue を Copilot に割り当て][assign-issue]ると、Copilot は進捗を追跡するドラフトプルリクエスト (PR) を作成し、環境をセットアップして作業を開始します。実行中または完了後にセッションを確認でき、提案された解決策のレビュー準備が整うと PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業が不要になるわけではありません。タスクの範囲を明確に定義します。AI ペアプログラマーと同じように段階的に作業し、学びながら試して調整してください。生成 AI を導入してもソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、その影響を確認しました。作業を割り当てる前に、リポジトリに含まれる次の指示ファイルを読み取り専用で確認します。

- `.github/copilot-instructions.md` の **Code standards** と TypeScript、TSDoc コメントに関する要件。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` frontmatter。`**/*.test.ts` に一致するファイルに適用されます。

「Code lacks documentation」Issue を割り当てた後、これらの指示による TSDoc、TypeScript 規約、ファイルコメントの影響を PR で確認します。

## Copilot cloud agent の開発環境を準備する

cloud agent は GitHub Actions の環境で動作します。作業前に実行するセットアップワークフロー `.github/workflows/copilot-setup-steps.yml` を作成すると、必要なツールと依存関係を準備できます。このラボでは Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザー、単一の Astro アプリ用にマイグレーションおよびシードされた SQLite データベースが事前に用意されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 各手順に必要な最小権限を設定してください。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL のみになりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"
      - name: Install JavaScript dependencies
        run: npm ci
      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリ用にローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub Actions ワークフローと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という 1 つのジョブを含みます。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブから手動実行できます。Copilot が実行するのを待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの不足や古さは、生産性や保守性を損ないます。ドキュメントの追加は、cloud agent に任せられる作業です。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。
   
   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい PR を開きます。表示されない場合は、少し待ってからページを更新します。
12. Copilot が todo リストを作成するまで待ちます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を構築し、先ほどのワークフローを実行し、プロジェクトを分析して、問題への最適な取り組み方を決めています。

13. Copilot が実行するタスクの一覧を確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態を確認するため、ページを再読み込みする必要がある場合があります。

16. ライブセッションをスクロールし、Copilot の作業を確認します。コードを調べて現状を把握し、計画を検討し、コードを作成する様子が表示されます。

todo リストの表示には数分かかることがあります。Copilot は環境を構築し、プロジェクトを分析して、タスクの進め方を決めています。処理中に次のタスクを割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent が作業している間に、デザインチーム向けの関連ゲーム機能も割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の要件を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めます。cloud agent は人間のソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後にレビューできます。非同期処理の間に、次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、コード生成環境をカスタマイズし、安全な操作と範囲を明確にした Issue の重要性を確認して、Issue を Copilot に割り当てました。cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使います。[cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。前の演習ではカスタム指示について確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
