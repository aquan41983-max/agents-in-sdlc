---
title: "演習 5 - GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に GitHub Copilot と複数のタスクに取り組みました。コードのドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を作成し、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装しました。Copilot の変更を確認し、必要に応じてフィードバックを伝えましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監視されずに非同期で作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みはコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内で、作業用の一時的な別環境を作成して実行されます。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、コードにドキュメントを追加する GitHub Copilot cloud agent のプルリクエスト (PR) をレビューします。GitHub.com の標準 PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。演習 2 で確認したカスタム指示ファイルから反映された TSDoc コメント、TypeScript 規約、ファイル先頭コメントを探します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちの表示があるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションでワークフローがキューに登録されたことを確認します。単一 Astro アプリのプロジェクト チェックがすべて成功するまで数分待ちます。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チーム メンバーと同じように PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的なため、必ず同じ結果になる具体的な依頼文は示せません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を短く説明するコメントを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、コードベースに追加したいドキュメントの変更を **@copilot** にメンションしてコメントします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は変更用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットが表示されます。
5. **Files changed** タブで変更をレビューします。

満足するまで繰り返し作業できます。準備ができたら、PR をドラフトからレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちの表示があるか確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションでワークフローがキューに登録されたことを確認します。単一 Astro アプリのチェックがすべて成功するまで数分待ちます。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストできます。Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、Codespace のターミナルで次を実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能が実装されました。cloud agent との作業を反復し、表示する関連ゲームの数を調整したり、ファイル先頭コメントや TSDoc コメントを追加したりできます。後者を依頼した Issue は、カスタム指示を追加する前に割り当てたことを思い出してください。これまでと同様に **Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、カスタム アクセシビリティ エージェントで実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで数分待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザーの設定を local storage に保存する方法。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちの表示があるか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションでワークフローがキューに登録されたことを確認します。プロジェクトのチェックがすべて成功するまで数分待ちます。
10. **省略可能:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動テストできます。Copilot が作成したブランチ名に置き換えて、Codespace のターミナルで次を実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイ コントラストとライト モードを切り替え、ページを再読み込みしても設定が保持されることを確認します。

カスタム アクセシビリティ エージェントは、アクセシビリティのベスト プラクティスに沿った機能実装を Copilot に促します。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に修正を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent には、実際のバックログ項目を割り当て、その間に別の作業を進める方法が適しています。この方法を身に付けるために、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に反映されることを確認しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、実行中のアクセシビリティ セッションを監視して誘導しました。
- Copilot のプルリクエストをレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスでは異なる環境から関連するシナリオを体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリで、次のアイデアを試してみましょう。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォール設定][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
