---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際はコンテキストが重要です。タスクの進め方や Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使って、Copilot に「何を」だけでなく「どのように」実行するかも伝えます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを限定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング規約、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムでの比較が難しいためです。このハーネスの後半で、指示ファイルが cloud agent の作成したプルリクエストに与えた影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ層には必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc コメントでコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを記載します。

指示ファイルを使って、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストです。コーディング規約、ライブラリ、コメントの書き方などを指定でき、リポジトリ全体または特定のファイル形式に適用できます。

- `.github/copilot-instructions.md` は、リポジトリに対するすべての依頼で Copilot に送られる単一の指示ファイルです。多くの依頼に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに適用する指示ファイルです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を含む `*.instructions.md` の両方を使用します。AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルもサポートされています。

### 指示ファイルを管理するためのベストプラクティス

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体の指針を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、ファイル形式やタスク固有の指針を記載します。
- 自然な言葉で明確に書き、望ましいコードと望ましくないコードの例を示します。

指示ファイルの作成について、この演習で詳しく扱うことはできません。サンプルプロジェクトの例は、実際の指示ファイルの構成を示しています。指示ファイルに唯一の正しい書き方はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイル群を用意します。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。操作は環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。テンプレートを探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトに含まれる指示ファイルを読みます。中心となる `copilot-instructions.md` と、各種タスク向けの `*.instructions.md` があります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` がリポジトリルートからの glob を指定することを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの指示ファイルとプロジェクト内の既存ファイルへのリンクを確認します。これにより大きな指示セットを再利用可能なファイルに分け、参照例を示せます。パスはリポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc とファイル先頭コメントの規則を追加します。

## リポジトリ標準を追加する

ここで編集するのは、プロジェクト全体に適用する規則の追加です。後の演習で Issue を割り当てたとき、cloud agent がこの指示を使います。

1. 前の演習で使った Codespace のターミナルから、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. 27 行目付近にある **Code formatting requirements** セクションを見つけます。ここにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則がないため、生成されたヘルパーにはドキュメントコメントがありません。既存の標準の直下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` は、ドキュメントコメントの規則など、プロジェクト全体の標準を置く場所です。特定の言語、ファイル形式、タスクには `*.instructions.md` を使います。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチにある指示ファイルを読み込みます。次の演習では `main` からブランチが作られるため、指示を `main` にマージしておきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは実際のプロジェクトのように進めるためブランチを作成しますが、`main` に直接作業する場合はブランチの手順を省略できます。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。後の演習で cloud agent が作成する PR に、この標準が反映されることを確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [指示ファイルやその他のリソースを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
