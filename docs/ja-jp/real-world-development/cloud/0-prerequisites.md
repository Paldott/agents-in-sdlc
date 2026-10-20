---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、環境を準備します。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集と cloud agent の成果のレビューに使う [codespace][codespaces] を起動します。

## ラボ用リポジトリをセットアップする

コード用のリポジトリを作成するには、[テンプレート][template-repository]からインスタンスを作成します。ラボに必要なファイルがそろった状態で作成されます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリページで **Use this template** を選択し、**Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリテンプレートの設定項目](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後の演習で参照します。

## codespace を作成する

次に、ラボの演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーでコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE が用意されています。

1. 新しく作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタン](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** を選択して新しい Codespace を作成します。

   ![新しい codespace の作成](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするより速く準備できます。作成中に GitHub Copilot のほかの機能を確認しても構いません。

> [!CAUTION]
> 後の演習で codespace に戻ります。それまでは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がそろった環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と作業する codespace の作成を開始しました。

## 次のステップ

次は cloud agent が従うカスタム指示を追加します。[演習 1「カスタム指示」][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
