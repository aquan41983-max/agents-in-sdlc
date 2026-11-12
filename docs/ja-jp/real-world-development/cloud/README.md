---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウド上で非同期に作業できるようにします。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を進められます。

この演習では、cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装させます。カスタム エージェントを確認して利用し、agents ダッシュボードからセッションを監視・誘導し、作成されたプルリクエストをレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | Issue を Copilot cloud agent に割り当てます |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使います |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して誘導します |
| [5. 反復][ex5] | レビュー | PR をレビューして Copilot の作業を改善し、次の手順を選びます |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] GitHub アカウントと有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プラン
- [ ] アカウントまたは Organization で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがない場合、認定学生は [GitHub Education][callout-student-plan-education] を通じて無料で GitHub Copilot を利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外の **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が Organization で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法は、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を確認してください。

## 開始する

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
