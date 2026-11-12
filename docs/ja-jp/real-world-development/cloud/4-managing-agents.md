---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot cloud agent に3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。実行内容を調べ、進行中のセッションに追加のガイダンスを伝えられます。

このレッスンでは、次のことを行います。

- cloud agent タスクを監視するため agents ページを確認します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを作成するようエージェントに依頼した後、チームはライト モードも同時に追加したいと考えました。サイトのスタイルと切り替え機能を更新しているので、この要望も含めるのが自然です。エージェントの作業を誘導し、ハイ コントラストに加えてライト モードも追加させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。コードベースのドキュメント更新、ゲームを変更する API の生成、Web サイトへのハイ コントラスト モードの追加など、Copilot に割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、Copilot 用環境の作成、MCP サーバーの設定、タスクの実行などが含まれる場合があります。

> [!NOTE]
> 表示される手順は、Copilot の作業状況と採用した方法によって異なります。

4. 右側のプルリクエスト (PR) ペインも確認します。ここでは PR と変更ファイルを確認できます。

## Cloud agent に追加の指示を伝える

進行中のタスクを確認したので、ハイ コントラスト モードに取り組んでいる Copilot にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードのセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページの cloud agent タスクとセッションの誘導ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に含めたことを確認します。

## Copilot に作業を続けさせる

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼を作業の流れに取り込みます。

今回も数分かかります。ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを確認しました。これは GitHub Copilot cloud agent のタスクを監視して誘導するための中心的な場所です。進行中および完了したタスクを追跡し、実行中の作業を確認し、途中でタスクの方向を変更したりガイダンスを追加したりできます。

このレッスンでは、mission control で cloud agent のタスクを監視し、進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot のプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
