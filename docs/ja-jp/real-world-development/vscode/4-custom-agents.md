---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[GitHub Copilot のカスタムエージェント][custom-agents-concept]は、開発ワークフローの特定タスクや分野に合わせた AI アシスタントです。リポジトリの `.github/agents` フォルダー (または `~/.copilot/agents`) に Markdown ファイルを置き、指示や専門知識を定義します。チームはアクセシビリティ、セキュリティ、テストなどの知識を再利用可能なエージェントにできます。ファイルには `name` と `description` を含む YAML frontmatter と、動作や指示を定める Markdown プロンプトを記載します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントは作業を担う役割、[エージェントスキル][agent-skills-concept]はツールと考えると区別しやすくなります。エージェントは独自のコンテキストウィンドウを持ち、作業中にスキルや他のエージェントを調整できます。このラボのアクセシビリティエージェントは、ガイドラインに沿ってサイトをレビューし、更新します。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず利用できるクラウドファンディングサイトを目指しています。現在のダークテーマは文字と背景のコントラストが不足して読みにくいという意見を受け、デザインチームが切り替え可能なハイコントラストモードを依頼しました。カスタムエージェントを使って機能を作成します。

## アクセシビリティ用カスタムエージェントを確認する

1. Codespace に戻ります。
2. ターミナルで `main` を更新し、アクセシビリティ作業用のブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. `.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

4. アクセシブルな Web サイト作成の責任、ベストプラクティス、HTML・CSS・JavaScript の例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント一覧に、`.github/agents` 内の各カスタムエージェントが表示されます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択します。

1. エージェント一覧から **Agent** を選択します。
2. 一覧を開き、**Accessibility agent** を選択します。
3. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Copilot がコードベースを調べ、変更を提案してプロジェクトファイルを編集する様子を確認します。ファイルごとの編集は差分として表示されます。
5. **Files changed** の表示で、エージェントが変更したファイルを確認します。

この作業には数分かかることがあります。Astro コンポーネント、CSS、関連テストなどが変更されます。次の演習で進行中の作業を確認し、追加指示を出します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]の定義を確認し、Copilot Chat のエージェントモードでアクセシビリティエージェントを使いました。次は[作業を監視して、同じセッションにライトモードを追加します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
