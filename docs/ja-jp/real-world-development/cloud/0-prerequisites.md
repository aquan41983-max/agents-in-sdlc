---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、[codespace][codespaces] を起動します。そこでは指示ファイルを編集し、cloud agent の成果をレビューします。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリは、[テンプレート][template-repository]から作成します。新しいインスタンスにはラボに必要なファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリ ページで **Use this template** を選択し、**Create a new repository** を選択してリポジトリをコピーします。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力](../../../_images/ex0-repository-settings.png)

4. ラボで後から参照できるよう、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## codespace を作成する

次に、codespace で演習を行います。[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数の言語、拡張機能、ツールをサポートする IDE を利用できます。

1. 新しく作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手動でインストールするよりも早く準備できます。その間、次に取り上げる GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件を用意します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業する codespace の作成を始めました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みましょう。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
