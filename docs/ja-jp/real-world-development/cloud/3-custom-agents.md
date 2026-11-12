---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、セキュア コーディングに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなど、チームの知識を再利用可能な形にできます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` にあるファイルで定義します。各ファイルは `name` と `description` を含む YAML frontmatter と、エージェントの振る舞い、専門性、指示を記述する Markdown プロンプトで構成されます。

### カスタム エージェントと agent skill の比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には概念上の共通点があります。どちらも主に Markdown ファイルで定義され、AI の作業方法を伝えます。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**だということです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業中にスキルや他のエージェントを調整するよう設計されています。このラボでは、アクセシビリティのカスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その際、プルリクエスト ワークフロー用やテスト実行・管理用のスキルを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作成する唯一の「正しい」方法はありません。AI の他の用途と同じように、環境やシナリオに合う方法を見つけるためにテストと反復を行います。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーから、文字と背景のコントラストが不十分で現在のダーク テーマが読みにくいというフィードバックがありました。デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能を生成するためにカスタム エージェントを使います。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは作成済みです。Copilot を導く方法を理解するため、内容を確認しましょう。

1. codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成時の中心的な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いを確認します。

## Issue を作成して割り当てる

Mission control は環境内のエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、監視し、追加のガイダンスを伝えられます。ハイ コントラスト モードの作成を Copilot に依頼しましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや領域に合わせた専門的な AI アシスタントです。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にし、Copilot の作業を導けます。

このレッスンでは、次のことを学びました。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して誘導します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [Organization でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
