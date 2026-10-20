---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

エージェントモードの GitHub Copilot は、コードベースを調べ、変更を提案し、ファイルを編集してコマンドを実行します。VS Code の Copilot Chat ではツール呼び出しとファイル編集をリアルタイムで確認し、差分ごとに変更を受け入れたり戻したりできます。エディターを離れずに追加プロンプトで作業を調整します。

この演習では、アクセシビリティエージェントの作業を監視し、提案された編集を確認して、ハイコントラストモードに加えてライトモードを追加するよう指示します。

## シナリオ

[演習 4][previous-lesson]で選択したアクセシビリティ用カスタムエージェントが、ハイコントラストモードを実装しています。作業中にデザインチームからライトモードも必要という依頼がありました。新しい会話を始めるのではなく、既存のセッションを案内して機能を追加します。

## Copilot の進捗を監視する

Copilot Chat には、エージェントが調べたファイル、実行したコマンド、提案した編集が表示されます。編集は差分として表示され、**Keep** と **Undo** で操作できます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. チャットをスクロールし、ツール呼び出し、ファイルの読み取り・編集、実行を求められたターミナルコマンドを確認します。
3. 編集案ごとに **Keep** で受け入れるか、**Undo** で戻してエージェントに再試行させます。
4. チャット上部の **Files changed** で、このセッション中に変更されたファイル全体を確認します。

> [!NOTE]
> ターミナルコマンドなどの許可を求められた場合、エージェントは **Allow** を表示して一時停止します。内容を確認し、実行してよいコマンドだけを承認してください。

5. エージェントがハイコントラストモードを実装し、変更をまとめるまで待ちます。

## ライトモードを追加するようエージェントに指示する

同じ会話を続けると、エージェントは変更済みのコンポーネント、スタイル、local storage の使い方をコンテキストとして保持できます。

1. Copilot Chat に次のプロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を調べ、ライトモードの追加を提案する様子を確認します。
3. 追加される差分を確認し、それぞれ **Keep** または **Undo** を選択します。
4. 意図しないリファクタリングなどがあれば、チャット入力欄の横にある **Stop** で停止し、追加の指示を送ります。

> [!TIP]
> 作業中のセッションへの追加指示は、やり直すより効率的です。たとえば、CSS の簡略化、設定パネルへの統合、両方の切り替えに使うキーボードショートカットの追加を依頼できます。

## 作業セットをレビューする

コミットする前に、エージェントが変更した内容を確認します。

1. VS Code の **Source Control** ビューを開きます。
2. 変更されたファイルを確認します。Astro コンポーネント、スタイル、関連テストが含まれることがあります。
3. 変更ファイルを開き、ARIA 属性、キーボード操作、セマンティック HTML、local storage による保存を確認します。
4. 問題がなければ **Source Control** パネルから変更をステージしてコミットします。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat のエージェントモードで進行中のセッションを監視し、ファイル編集をレビューして、変更を受け入れまたは差し戻し、追加プロンプトで作業範囲を広げました。次はアクセシビリティの作業を[プルリクエストとして公開し、変更をレビューします][next-lesson]。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードを使う][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
