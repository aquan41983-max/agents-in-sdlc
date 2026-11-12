---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤としたデスクトップ アプリケーションです。エージェント主導の開発に必要な機能を、1つの専用ワークスペースにまとめています。複数のエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas を共有し、GitHub Issue とプルリクエストをネイティブに管理できます。**Agent Merge** は、rebase、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを通じてプルリクエストを進めます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から開始し、**Plan** モードでアプローチを定義して、**Autopilot** モードで構築した後、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) カスタム エージェントを作成して使い、要件、カバレッジ、検証の根拠を確認します。
8. フィルタリングの変更全体をレビューし、2つ目の PR に Agent Merge を使います。
9. Canvas が利用できるか確認した後、Database Explorer を使い、リポジトリに保存するトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップに集中できるよう、PR は2つ作成します。星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの変更です。それぞれ更新済みの `main` から始めます。フィルタリングと品質のワークフローでは、1つのセッション、worktree、ブランチを共有し、各ツールでの作業を積み重ねます。最後の Canvas 演習はそのセッション内で行い、PR の流れを繰り返さずに共有スペースの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトのコピーを作成します |
| [1. Copilot app をインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive でレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認・更新して、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変えて実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize から MCP を設定し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選び、最終的な検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストをレビューし、2つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ Canvas を作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを確認します |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。これは Copilot CLI ポリシーとは別です。

## はじめる

**[レッスン 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
