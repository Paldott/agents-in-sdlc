---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

単純な更新でも複数ファイルの変更やテストが必要になることがあります。必要なファイルを探し、変更し、テストを実行して、問題をデバッグする作業を GitHub Copilot Agent Mode に任せられます。Agent Mode はプロジェクトを調べ、複数のファイルを更新し、テストを実行して、検出した問題を修正します。

> [!NOTE]
> Agent Mode と cloud agent は異なる体験です。Agent Mode は IDE 内で対話しながら作業し、cloud agent は Issue とプルリクエストを使ってチームの一員のように非同期で作業します。

この演習では、Copilot Agent Mode がプロジェクトを調べて関連ファイルを見つけ、UI とデータ層をまたぐ機能を実装する方法、生成された変更とテストをマージ前にレビューする方法を確認します。

## シナリオ

ゲームの一覧が増えているため、利用者がカテゴリやパブリッシャーで絞り込めるようにします。前の演習で出版社ヘルパーを追加したので、Agent Mode を使って残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディングサイトです。ゲーム一覧と詳細ページを表示します。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント UI フレームワークはありません。データアクセスヘルパーは `src/lib/` にあり、インタラクティブ機能は DOM API を使う小さな Astro `<script>` で実装します。

1. Codespace に戻り、前の演習で更新した `main` にいることを確認します。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開き、新しいブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. サイトを起動します。

   ```bash
   npm run dev
   ```

4. Astro 開発サーバーが起動したら、ターミナルに表示された `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択します。Codespace の localhost リンクは `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. 一覧ページでゲームを確認し、ゲームを選択してタイトル、説明、パブリッシャー、カテゴリが掲載された詳細ページを確認します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択します。演習で指定がある場合はモードとモデルも選びます。

1. Chat ビューのエージェント一覧から **Agent** を選択します。
2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。
3. 次のプロンプトで GitHub リポジトリのバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue 一覧の取得を求められたら **Continue** を選択し、表示された優先順位を確認します。

> [!CAUTION]
> このワークショップで Claude Sonnet 4.5 を使ったため、例との一貫性を保つ目的で記載しています。モデルの推奨を意味するものではありません。LLM は確率的に動作するため、出力は異なる場合があります。

## 指示ファイルを確認する

コード生成を依頼する前に、`.github/instructions/ui.instructions.md` を開き、アーキテクチャ、コンポーネント設計、テスト容易性、アクセシビリティの指針と、Astro・Tailwind CSS・Drizzle の指示へのリンクを確認します。

## フィルター機能を実装する

フィルターには、データ層のロジック、テスト、一覧ページの UI 更新が必要です。

1. 現在の会話を続けるか、**New Chat** で新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択します。
3. エージェントが **Agent**、モデルが **Claude Sonnet 4.5** であることを確認します。
4. バックログの関連 Issue に基づき、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Copilot がデータ層、UI、テストを調べて変更する様子を確認します。
6. テスト実行を求められたら **Continue** を選択します。作業中にテストが失敗することがあります。Copilot はコードとテストを反復します。
7. 生成コードをレビューします。AI が生成したコードは必ず確認してください。
8. ブラウザーに戻り、新しい機能を試します。
9. 動作とコードを確認したら Copilot Chat の **Keep** を選択します。

## まとめと次のステップ

Copilot Agent Mode が UI とデータ層に機能を実装し、関連ファイルを調べて変更を調整する方法を確認しました。次は[Playwright MCP サーバーで機能をテストし、PR を作成します][next-lesson]。

## リソース

- [Copilot の ask、edit、agent モード][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
