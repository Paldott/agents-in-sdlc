---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方や Copilot に知ってほしい背景情報がある場合は、参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関するガイダンスを提供できます。

この演習では、リポジトリのカスタム指示とパスを限定した指示ファイルを確認し、既存の指示で出版社ヘルパーを作成します。その後、`.github/copilot-instructions.md` にリポジトリ標準を追加して同じプロンプトを再実行し、指示による変化を確認します。最後に、指示とフィルター機能の基礎を `main` にマージします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot は確率的に動作するため、この演習では文字単位で同じ出力を得るのではなく、指示を更新した後の傾向を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys では、データ層に単体テストが必要であること、UI をダークモードにして現代的な見た目にすること、コードに TSDoc コメントを付けること、各ファイルの先頭に役割を説明するコメントを記載することが求められています。指示ファイルを使って、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体または特定のファイル形式に適用します。

- `.github/copilot-instructions.md` はリポジトリ内のすべての依頼に適用される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、ほとんどの依頼に関連する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro など特定の言語や、UI コンポーネントや単体テストの作成など特定のタスク向けの指針を記載します。

> [!NOTE]
> IDE の指示ファイルは Copilot Chat のコード生成に適用されますが、コード補完や次の編集候補には適用されません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ指示ファイルを利用します。AGENTS.md や CLAUDE.md などの形式もサポートされています。

### 指示ファイルを管理するためのベストプラクティス

サンプルプロジェクトの指示ファイルは、代表的な構成例です。

- `copilot-instructions.md` は構築するものの説明、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針に絞ります。
- `*.instructions.md` は単体テスト、Astro コンポーネント、データ層など、ファイル形式やタスクに固有の指示に使います。
- 自然な言葉で明確に書き、コードの適切な例と不適切な例を示します。

指示ファイルや AI の使い方に唯一の正解はありません。実際に試して、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイル群を用意します。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。操作は環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。テンプレートを探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる中心的な `copilot-instructions.md` と、タスク別の `*.instructions.md` をエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フィールドがリポジトリルートを基準に対象を決める glob であることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。これにより大きな指示セットを再利用可能なファイルに分け、コード生成時の例を Copilot に示せます。パスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** には標準が記載されていますが、コード内ドキュメントの規則はありません。次に TSDoc とファイルコメントの規則を追加します。

## 変更用ブランチを作成する

1. Codespace に戻り、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。

1. 前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを適用する様子を確認します。ヘルパーファイルとテストの両方が変更される場合があります。
6. 提案されたヘルパーが、データ層の規約に沿って `db` クライアントを最初の引数に受け取り、型付きの出版社配列を返す関数になっていることを確認します。この規約は `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` に由来します。
7. 提案コードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot が最初からコメントを追加する場合もあります。その場合も、指示更新後に一貫性が高まることがこの演習の目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝える場所です。コーディング標準を記録してコード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近の **Code formatting requirements** セクションを見つけます。生成ヘルパーにドキュメントコメントがなかったのは、この規則がまだないためです。
3. 既存の標準の直下に、ファイルコメントヘッダーと TSDoc コメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体の指針用の `.github/copilot-instructions.md` と、特定の言語・ファイル形式・タスク用の `*.instructions.md` に分けられます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択します。
2. `src/lib/publishers.ts` を開きます。
3. 指示を更新する前と**同じプロンプト**を送信します。
4. ファイル先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使うコードに適用しました。

## 指示とブランチをプッシュする

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. `git status` を実行し、指示の更新とヘルパーの変更が存在することを確認します。
3. 指示とヘルパー、および Copilot が追加で変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue と関連付けた PR を作成します。GitHub ツールを使って Copilot に Issue の検索、PR の作成、マージを依頼します。

> [!NOTE]
> Copilot が GitHub 上で操作するのはこの演習が初めてです。プロジェクトのテンプレートには GitHub ツールがすでに接続されています。初回利用時に VS Code から GitHub へのサインインを求められたら、画面の指示に従います。接続に使う Model Context Protocol (MCP) は後の演習で扱います。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで、指示ファイル更新の Issue を探して、指示の更新と今後のフィルター作業に使う出版社ヘルパーの両方を説明した PR を現在のブランチから作成するよう依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を始めることを確認します。
5. GitHub 操作の許可を求められたら、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージしてローカルブランチを `main` に戻し、最新の変更を取得するよう依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの許可を求められたら、内容を確認して適切に **Allow** を選択します。

これで GitHub Copilot の支援を受けて PR を作成し、マージできました。

## まとめと次のステップ

指示ファイルが Copilot にコンテキストを伝える方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示を使ったコード生成を依頼し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーをコミットし、`main` にマージしました。

次は[エージェントモードで指示を機能全体に適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot の指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [指示ファイルを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
