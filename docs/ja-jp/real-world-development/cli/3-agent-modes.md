---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存フローに沿って動作し、適切な方法で適切なものを構築できるようにします。このレッスンから、典型的なエージェント主導の開発プロセスに沿って、Issue を起点に新機能を作成し、コードの妥当性と機能の動作を確かめ、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使用します。通常はファイルの種類ごとに別のブランチや PR を使いますが、ここでは主な概念に集中できるよう簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動検証します。

機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリとパブリッシャーで絞り込める必要があります。バックログの Issue には機能が説明されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておく詳細があります。Plan モードで決定し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、いっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードがレビューされ、継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの場合、上記の流れを調整したものになります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントは先に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了したと判断するまで自律的に作業を繰り返します。

まず Plan モードで計画をレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルタリング機能の作業を始める前に、Codespace に戻ってリポジトリとターミナルの準備をします。

1. Codespace に戻ります。停止中の場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることに気づくでしょう。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能の実装やタスクを Copilot に任せる前にアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切なアプローチを判断して、計画を作成します。

3. フィルタリングの動作について質問されたら、自分の希望に基づいて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様の名前の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って作業を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。まず計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力して、フィルタリングの実装とテストを調べます。
3. 結果を Issue および計画時の決定と比較します。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認できました。次はアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャーのフィルタリング、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]、コードが文書化された規約に従うようにしましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
