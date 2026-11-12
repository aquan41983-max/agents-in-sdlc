---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要求などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業を終えると、人間がレビューできるドラフト PR を作成します。面倒な作業を委任し、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作成した開発者はドキュメントを十分に作成していません。まず、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

また、デザイン チームはゲームの発見方法を改善したいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。完成度の高い実装ではなく、UX の受け入れテストに使える状態を求めています。この作業はブロッカーですが、優先度がより高い別の Issue があります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業を進める間に非同期で対応できます。完了後にエージェントの変更をレビューします。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取り組み始めます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業にも複雑さがあるため、[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、実験し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後で結果に反映されている内容を探します。

リポジトリの GitHub Web UI または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認し、TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript 規約、ファイル先頭コメントがあるかを確認します。後の演習でも PR のレビュー時に取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、通常、ツールや依存関係を整える特定の環境が必要です。SWE と同様に作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は [GitHub Actions][github-actions] を使って環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップ ワークフロー][setup-workflow]で、作業前に必要なツールや依存関係を用意できます。ラボ用の設定は事前に用意済みです。Node.js、プロジェクト依存関係、E2E テスト用のブラウザー、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このファイルは他の GitHub Actions ワークフローと同様ですが、ポイントがあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` を使うと、リポジトリの **Actions** タブから手動で実行できます。Copilot が動かすのを待たずに、ワークフローの成功をテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメント作成に役立つため、cloud agent に割り当てるのに適した Issue です。エージェントはバックグラウンドでドキュメントを作成します。後の演習で変更をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。すぐに表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、課題への最善のアプローチを決めています。

13. TODO リストと、実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを確認します。
15. **View session** を選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるよう、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして、Copilot がコードを調査し、状態を理解し、アプローチを考えてコードを作成する様子を確認します。

処理には数分かかる場合があります。cloud agent の主な目的は、非同期でタスクを実行して別の作業に集中できるようにすることです。2つ目のタスクを割り当ててから、アプリに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

作業を分担し、Copilot が対応している間に別の作業を進められることも、cloud agent の利点です。デザイン チーム向けに関連ゲームのセクションを追加する間に、別の作業を進められます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。cloud agent は SWE と同じように作業するため、常時監視する必要はありません。完了後にレビューします。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。Issue を Copilot に割り当てると、非同期に処理できます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などに利用できます。

次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。前のモジュールではカスタム指示を確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
