---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しましたが、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えます。このレッスンから、Issue を起点に機能を開発し、コードの妥当性と期待どおりの動作を確認して、プロジェクトにマージするまでの一般的なエージェント主導のプロセスを進めます。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを使います。通常、種類の異なるファイルを扱う作業ではブランチや PR を分けますが、ここでは主要な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この後、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズして、MCP による検証と QA エージェントの作成を行い、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリやパブリッシャーでゲームを絞り込めるようにします。バックログの Issue に機能要件は記載されていますが、複数のカテゴリを組み合わせる方法など、実装前に合意が必要な点もあります。Plan モードで決定し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。一般的な流れは次のとおりです。

1. 作業内容を記した登録済みの Issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって細かな手順は異なりますが、多くの場合、上記の流れを基本とします。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ審査を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードが切り替わります。

- **Interactive**: エージェントが変更を提案し、開発者の入力を待ちながら共同作業します。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認してから、Autopilot で実装します。

## Issue から始める

フィルター機能を始める前に、Codespace とリポジトリの準備を整えます。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトに `#7` (または同様の番号) が追加されます。`#` を使うと GitHub の Issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、機能やタスクの実装を Copilot に任せる前に、進め方を定義できます。複雑な作業では、計画に時間をかけると効果的です。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な進め方を検討して計画を作成します。

3. フィルターの動作について質問されたら、希望する動作を回答します。回答に唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画の作成とレビューが終わったら、Autopilot で実装します。Autopilot は、完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前のオプションを選択します。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係の不足やポートの競合が報告された場合は、チェックの完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードも、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して動作を確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 変更内容を Issue と計画時の決定に照らして確認します。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードが適切に見えても、実際に動作するとは限りません。以前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、パブリッシャー、および組み合わせで絞り込みます。
4. リセットと該当結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能の構築とレビューを行いました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使い][next-lesson]、コードが文書化された開発慣行に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
