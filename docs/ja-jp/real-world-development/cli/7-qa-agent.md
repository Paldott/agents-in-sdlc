---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、カスタムエージェントと指示、スキル、MCP ツールの連携を確認し、再利用可能な品質保証 (QA) プロファイルを作成して、フィルター Issue に対する調査結果を確認します。

## シナリオ

Tailspin Toys では PR の作成前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫した方法で確認したいと考えています。カスタムエージェントを使って QA プロセスを調整し、再利用可能なレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルに目的、指示、使用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義します。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらを使って要件を評価し、結果を報告する方法を Copilot に指示します。

## QA プロファイルを作成する

機能 PR を作成する前に、チェック内容と作業範囲を定義する再利用可能な QA プロファイルを作成します。

1. Codespace に戻り、フィルター会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェント作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、意図したワークフローと制約が反映されていることを確認します。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須で、`name` は省略可能ですが、表示名として指定できます。
3. 指示を読み、要件から始め、リポジトリ指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. 根拠を示して報告すること、実装コードの変更前に許可を求めること、コミットや PR 作成を行わないことを確認します。
5. 不足があれば、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトエージェントを読み込みます。プロファイル作成後、同じフィルター会話を再開し、QA を選択します。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後のエージェントが一覧に表示されない場合でも、上記のコマンドで有効にできます。

2. QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を終えたら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、そのレビュー結果を確認しました。次は[機能 PR に変更をまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
