---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前にブラウザーで動作を確認します。**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーでテストを依頼してから、ブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) と MCP サーバーの役割を確認し、Playwright MCP をワークスペースに追加して、ブラウザー上でフィルターをテストします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、Copilot が外部ツールやサービスと通信するための標準です。MCP サーバーは AI エージェントと外部 API やローカルツールを仲介し、情報へのアクセスや操作を可能にします。[GitHub MCP Server][github-mcp-server] は GitHub リポジトリ、Issue、プルリクエストを扱い、[Playwright MCP Server][playwright-mcp-server] はブラウザーを自動操作します。[MCP registry][mcp-registry] には、ほかのサーバーも登録されています。

> [!CAUTION]
> MCP サーバーも、ほかの依存関係と同じように扱います。利用前にソースコードと発行元を確認し、セキュリティ上の影響を検討してください。信頼できるサーバーだけを使い、機密情報や操作へのアクセス付与に注意します。

## MCP の設定を確認する

`.vscode/mcp.json` で、この VS Code ワークスペースの MCP サーバーを設定します。

![コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールを含むエージェントモードの仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` サーバーが設定済みであることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

テンプレートに含まれるリモート GitHub MCP サーバーを使うため、ローカルへのインストールは不要です。VS Code が HTTP で接続し、Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

1. `.vscode/mcp.json` の `github` と並べて `playwright` を追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

VS Code はローカルサーバーの `playwright` を `npx @playwright/mcp@latest` で起動します。`--headless` は画面を表示せずにブラウザーを実行するオプションで、デスクトップ画面のない codespace で必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストに Playwright を使うため、必要なブラウザーは通常インストール済みです。不足が報告された場合は、Copilot に `npx playwright install chromium` の実行を依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動し、信頼を確認します。

1. `.vscode/mcp.json` の `playwright` の上に表示される **Start** を選択します。
2. 信頼の確認が求められたら、設定を確認してサーバーを信頼します。

手動起動は任意です。省略した場合は、次の手順で Copilot がツールを初めて使うときに起動と確認が行われます。

## フィルター機能をテストする

Playwright MCP はページの[アクセシビリティツリー][playwright-mcp-server]を使います。ラベル付けされたボタンやリンクなどを扱うため、機能確認と簡単なアクセシビリティ確認を兼ねられます。

1. アプリが起動していることを確認し、Copilot に次のプロンプトを送ります。

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. Copilot のレポートを Issue の受け入れ基準と照らし合わせます。問題があれば追加で質問するか、PR 作成前にコードの修正を依頼します。

> [!NOTE]
> テストにはアプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は再起動します。ブラウザーが不足している場合は `npx playwright install chromium` を実行してから再試行します。

## ブランチを公開してプルリクエストを作成する

1. Codespace の **Source Control** パネルで Copilot の変更を確認します。
2. **+** を選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを作成します。
4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

1. Copilot Chat パネルで **New Chat** を選択します。
2. エージェント一覧で **Agent** を選択します。
3. 次のプロンプトで Issue に関連付けた PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. **Continue** を選択して Copilot の操作を許可します。GitHub ツールを初めて使うときは、認証が必要になる場合があります。
5. Copilot が Issue を探して PR を作成したら、リンクを開いてレビューします。まだマージしないでください。

## まとめと次のステップ

MCP の概要と拡張方法を確認し、Playwright MCP でブラウザーから機能をテストして、ブランチを公開し PR を作成しました。次は[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
