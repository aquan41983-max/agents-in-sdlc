---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質のワークフロー全体を1つのセッションとブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学びます。
- 機能の PR 全体と検証の根拠を確認します。
- レビュー後に Agent Merge を承認し、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の作業を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys は、マージの承認を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、アプリのセッションがプルリクエストを読み込み、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase など、マージを妨げる問題に対処します。GitHub が許可するとマージし、バックグラウンドで実行されます。アプリを再起動しても継続し、プルリクエストがマージされると自動的に停止します。

ここまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、コードの編集やマージには明示的な承認が必要です。マージ権限を与える前に、許可される操作と作業をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが終わったら、Agent Merge に PR プロセスを管理させます。

1. エージェント ピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの選択だけでは始まりません。

Agent Merge は次の作業を行います。

- タイトルと説明を含むプルリクエストを作成します。
- Issue からセッションを開始した場合は、説明の本文で関連 Issue に言及します。
- rebase を行い、ターゲット ブランチとのマージ競合に対処します。
- CI を監視して、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックを監視し、コメントに対応するため変更します。
- すべてのチェックが成功した後に PR を自動的にマージするよう設定できます。

すべてのチェックに成功した後で、Agent Merge に PR をマージさせます。

5. マージを許可する前に、PR の差分と検証の根拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進行状況を確認し、報告されたブロッカーに対応します。次に進む前に、PR の状態が **Merged** であることを確認します。マージ権限が有効になったことやエージェントが実行中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前に、それらのブロッカーを解消してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の作業を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学びました。
- 機能の PR 全体と検証の根拠を確認しました。
- レビュー後に Agent Merge を承認し、PR がマージされたことを確認しました。

次は[既存のキャンバスを使ってトリアージ キャンバスを作成し][next-lesson]、エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
