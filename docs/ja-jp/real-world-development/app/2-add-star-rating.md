---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットも試しました。次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がありますが、ホームページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントに対象を絞った小さな変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲーム カードに表示されるのは、タイトル、カテゴリ、パブリッシャー、説明だけです。ウォームアップとして、各カードに既存の評価を表示させます。最初のセッションにぴったりの、小さく独立した変更です。

## セッションの構成

**セッション**は、個別の分離されたワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、あるセッションで機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーでリポジトリごとにまとめられ、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルの調査や編集時に行われる**ツール操作**、差分を確認できる**変更ファイル一覧**が表示されます。

## セッションを始めて変更を依頼する

新しいセッションを始め、プロジェクトを確認して機能の実装に取りかかります。[アプリのセットアップ][prior-lesson]で GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して変更を依頼します。

1. GitHub Copilot app を開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトを送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押してプロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離コピーである新しい worktree を作成し、プロジェクトを調べて新機能に必要なファイルを見つけ、コードを作成します。Copilot app で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリ右上の **Toggle review panel** を選択し、表示されたパネルで **Changes** を選びます。変更されたファイルと差分が表示されます。

   ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` に、次のようなコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` と表示します。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。似た動作になっていることを確認してください。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動実行したチェックの結果を確認します。数値の `starRating` と、`null` の場合の代替表示をテストしていることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新された UI を確認します。

> [!TIP]
> キャンバスは Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンでは独自のキャンバスを作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトを送信し、アプリを起動してブラウザー キャンバスで開くよう依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーが表示されます。
3. 評価済みのゲーム カードに5段階の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで開発サーバーを停止してブラウザー キャンバスを閉じます。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードをコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、案内に従って認証します。
3. PR を作成する前に該当する Issue が必要だと Copilot が説明した場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー パネルで PR を開きます。必要に応じて内容を確認します。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、その構成を学びました。
- ゲーム カードに小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を作成し、チェックをレビューして明示的にマージしました。

次は、[フィルタリングの Issue から開始し、Plan と Autopilot の各モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
