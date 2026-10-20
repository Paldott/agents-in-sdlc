---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` に Markdown ファイルを置き、Copilot が特定の作業を効果的に行えるよう、対象を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を提供します。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング慣行に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントがあります。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に置く Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には役割の重なりがあります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。両者を区別するには、**カスタムエージェントは作業者、スキルはツール**と考えると分かりやすくなります。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業中にスキルや他のエージェントを調整できます。このラボのアクセシビリティエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その際、プルリクエストのワークフロースキルやテスト実行・管理スキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。ほかの AI の使い方と同じように、環境とシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントの定義方法と、タスクの割り当て方を確認します。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるように取り組んでいます。現在のダークテーマは、文字と背景のコントラストが不足し、読みにくいという利用者の意見が寄せられています。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。アクセシビリティエージェントに機能を作成させます。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認して、Copilot への指示を把握します。

1. codespace に戻ります。
2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを確認します。アクセシブルな Web サイトのコード作成に関する責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript の例、よくある問題が記載されています。

## Issue を作成して割り当てる

1. リポジトリを開きます。
2. **Issues** タブを選択し、**New issue** を選択します。
3. **Blank issue** を選択します。
4. **Title** に `Add high contrast mode to website` と入力します。
5. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. **Create** を選択します。
7. 右側の **Assign to Copilot** を選択します。
8. カスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントと Accessibility が選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

9. **Assign** を選択します。

## まとめと次のステップ

[GitHub Copilot のカスタムエージェント][custom-agents]は、特定のタスクや分野に合わせた AI アシスタントです。チームの知識や標準を再利用可能なエージェントにまとめられます。この演習では、カスタムエージェントの定義方法と、タスクの割り当て方を確認しました。

Copilot がハイコントラストモードを実装している間に、次は[エージェントセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
