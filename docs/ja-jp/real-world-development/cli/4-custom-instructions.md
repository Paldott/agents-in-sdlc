---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合、その情報を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、コードに何を求めるかだけでなく、どのように構成するかも伝えられます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに反映します。

このレッスンでは、リポジトリ指示とパスを限定した指示ファイルがエージェントに届く仕組みを確認します。また、指示ファイルを更新してコーディング標準を適用し、コードへの影響を確認します。

## シナリオ

Tailspin Toys の開発ガイドラインでは、コメントで意図や自明でない判断を説明すること、`db/` と `src/lib/` のエクスポート関数に目的・パラメーター・戻り値を記した TSDoc/JSDoc を付けること、Astro コンポーネントの `Props` を説明すること、既存の書式と lint の指針を維持することが求められています。

指示ファイルを使うと、コーディングスタイルや要件を Copilot に伝えられます。推奨する規約、ライブラリ、コメントの書き方などを指定でき、リポジトリ全体または特定のファイル形式に適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリのすべての依頼で Copilot に送信される単一の指示ファイルです。多くの依頼に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスク向けのガイドラインを記載します。

> [!NOTE]
> 指示ファイル形式のサポートは環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示はリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示セットを再利用可能な小さなファイルに分け、Copilot に参照例を示せます。

## チームのガイドラインに合わせて指示ファイルを更新する

次に、TypeScript に TSDoc コメントを追加するよう、`.github/copilot-instructions.md` を更新します。

1. **Code formatting guidance** セクション (35 行目付近) を見つけます。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリ指示を読み込みます。更新した指示を読み込ませるため、フィルター会話を終了して再開します。

1. フィルター会話が開いている場合は `/exit` を入力します。会話は自動的に保存されます。
2. リポジトリルートの `game-filters-cli` ブランチから、保存済み会話を再開します。

   ```bash
   copilot --resume
   ```

3. 選択を求められた場合はフィルター会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
4. Copilot に更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を実行して変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリ指示とパスを限定した指示ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [指示ファイルなどのリソースを集めた Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
