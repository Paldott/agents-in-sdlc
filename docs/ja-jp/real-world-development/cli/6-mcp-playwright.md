---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれた機能を超えるツールやサービスを利用できます。

このレッスンでは、Model Context Protocol (MCP) と Copilot CLI での使い方を確認し、必要に応じて Playwright MCP サーバーを追加して、エージェントにブラウザーでフィルター機能を調べてもらいます。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには実際に UI を操作する方法もあります。Copilot が利用者のように Web サイトを操作できるようにし、変更が期待どおりに動作するか確かめます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP サーバーはエージェントと外部ツールの橋渡しをします。たとえば [GitHub MCP Server][github-mcp] は GitHub リポジトリ、Issue、プルリクエストを管理する API を提供し、[Playwright MCP Server][playwright-mcp-server] は Playwright によるブラウザー操作を提供します。GitHub の [MCP レジストリ][mcp-registry]で、ほかのサーバーも確認できます。

> [!CAUTION]
> MCP サーバーも、ほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP サーバーを追加する

Copilot CLI に Playwright MCP が設定されていない場合は、Web サイトを利用者のように操作できるよう追加します。

> [!NOTE]
> Playwright MCP がすでに設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを実行し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを入力します。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して保存します。
7. <kbd>Esc</kbd> を押してダイアログを閉じます。

## Playwright で機能を調べるよう Copilot に依頼する

Playwright MCP サーバーで、ブラウザーから機能を検証します。

1. Copilot に次のプロンプトを入力します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に選択するため、明示的に指示しなくても構いません。サーバー名を示すと、より安定して利用できます。

2. Copilot がブラウザーで検証を進める様子を確認します。
3. 結果を読み、期待したとおりに動作しているか確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP を使って、実際のブラウザーで機能を調べました。次はスキルとブラウザーツールを組み合わせる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
