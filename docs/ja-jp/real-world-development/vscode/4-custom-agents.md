---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、セキュア コーディングに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなど、チームの知識を再利用可能な形にできます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` にあるファイルで定義します。各ファイルは `name` と `description` を含む YAML frontmatter と、エージェントの振る舞い、専門性、指示を記述する Markdown プロンプトで構成されます。

### カスタム エージェントと agent skill の比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には概念上の共通点があります。どちらも主に Markdown ファイルで定義され、AI の作業方法を伝えます。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**だということです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業中にスキルや他のエージェントを調整するよう設計されています。この演習では、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その際、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルなどを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作成する唯一の「正しい」方法はありません。AI の他の用途と同じように、環境やシナリオに合う方法を見つけるためにテストと反復を行います。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、現在のダーク テーマは文字と背景のコントラストが不十分で読みにくいという指摘がありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントに機能を生成させましょう。この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは作成済みです。Copilot を導く方法を理解するため、内容を確認しましょう。

codespace に戻り、ターミナルを開いて、フィルタリング PR とは分けてアクセシビリティ作業用の新しい `main` ブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成時の中心的な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いを確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` で定義されたカスタム エージェントが表示されます。エージェントを選ぶと、そのエージェントの指示とツールに合わせて Chat セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントのドロップダウンから、**Agent** を選択します (まだ選択されていない場合)。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェントのドロップダウンを選択します。選択中のエージェントが表示され、既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat はコードベースを調べ、変更案を提示して、プロジェクト ファイルを編集します。編集ごとにファイル パスと差分が Chat 内に表示されます。
6. エージェントが変更した作業ファイルを確認できるよう、**Files changed** の表示が更新されます。

> [!NOTE]
> 実際のリポジトリが変更されるため、この処理には数分かかる場合があります。Astro コンポーネント、CSS、関連テストなどが更新されます。

次の演習では、この作業の進行状況を確認し、途中で指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや領域に合わせた専門的な AI アシスタントです。カスタム エージェントを使うと、チームの専門知識と標準を再利用可能な形にし、Copilot の作業を導けます。

このレッスンでは、次のことを学びました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して調整し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
