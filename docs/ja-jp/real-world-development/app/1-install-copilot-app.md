---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールしてサインインし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使用するデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。作業をすべて自分で行うのではなく、複数のエージェントを個別の分離されたワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログなど、ワークスペースを確認します。
- クイックチャットを試し、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを導入しています。Copilot app では、Issue を選び、エージェントを実行し、変更をレビューして pull request をマージするまでの作業を一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の案内に従って認証します。データ レジデンシーを使用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択をスキップし、後からプロジェクトを追加することもできます。
6. 求められたらテーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに表示されている場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. サイドバーにリポジトリが表示されていることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとまり、その下に個別のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、サイドバーにある主な領域を確認しましょう。

- **New** - 新しい Copilot チャット セッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を使って PR を表示します。アプリを離れずに PR の参照や絞り込み、CI 状態の確認、レビューができます。
- **Issues** - Issue を表示し、参照や絞り込みをしたり、Issue からセッションを開始したりします。
- **Automations** - スケジュールまたはオンデマンドで実行するエージェント タスクを保存します。やることリストや定期的な保守などを任せるのに便利です。これは追加の演習ではなく、振り返りの次のステップとして紹介します。
- **Customize** - MCP サーバー、プラグイン、スキルなどを追加します。Playwright MCP の設定で使用します。
- **Chats** - 独自のブランチやワークスペースが不要な質問やブレインストーミングのための簡易的な会話です。最初のチャットを始めると表示されます。表示されない場合は **Sessions** の横にある **+** を選択して、最初のチャットを開始します。
- **Sessions** - リポジトリ フォルダーと、その中のエージェント セッションをまとめます。リポジトリ名 (例: `tailspin-toys`) を選択するとリポジトリ ビューを開き、セッションを選択すると会話に戻ります。セッションごとに分離されたワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。最初のセッションは星評価の追加で始めます。

ワークショップを進めながら、ワークスペースをさらに確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能か知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

ほとんどのプロジェクトにはバックログがあり、Tailspin Toys も例外ではありません。テンプレートから作成されたバックログを確認しましょう。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリ フォルダーを選択します。フォルダー内の個別セッションではありません。リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して詳細を確認します。各 Issue はエージェント セッションの開始点にもなります。簡単な最初の変更が終わったら、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は、Copilot app に追加したリポジトリの項目だけに自動的に絞り込まれます。他のリポジトリの作業項目を表示するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。**quick chat** では、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、一時的な質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されない場合は、**Sessions** の横にある **+** で最初のチャットを開始すると表示されます。
2. アプリ自体のセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。セッションごとに分離された git worktree が使われるため、変更を競合させずに複数のエージェントを並行して動かせることが分かります。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールしてプロジェクトを接続し、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースと **Issues** のバックログを確認する。
- クイックチャットで一時的な質問をする。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
