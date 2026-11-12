---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリの小さな更新でも、通常は複数ファイルの編集やテスト実行が必要です。開発者は必要なファイルを探し、変更し、テストを実行し、不具合を調べ、見落としたファイルを更新する作業を繰り返します。

そのようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内で、より自律的に作業します。プロジェクト構成を調査し、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で作業し、すばやいフィードバックとやりとりを可能にします。cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- UI とデータ レイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode でデータ レイヤー、UI、テストの残りを実装します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

Web サイトは、開発者向けテーマのボード ゲームを扱うクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示する単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を介してローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動できます。

1. codespace に戻ります。前の演習後は `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Web サーバーを実行中の codespace へのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、次の主な機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを設定すると GitHub Issue のバックログも作成されています。Copilot に項目を表示させましょう。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。Agent は複数ファイルの変更を自律的に計画・実行し、ターミナル コマンドやツールを使います。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。体験を揃えるためですが、LLM は確率的に動作するため、提案は記載内容と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信し、バックログについて Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つと考える項目を優先順位付けします。

## 指示ファイルを確認する

コード生成を開始する前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトへの機能追加方法を説明する [ユーザー インターフェイス (UI) ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャ、コンポーネント設計、テスト容易性、アクセシビリティの原則、ファイルの種類別の指示 (Astro、Tailwind CSS、Drizzle) へのリンクが含まれています。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。大きな指示を再利用可能な小さなファイルに分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングには、少なくとも次の3種類の変更が必要です。

- データ レイヤー (`src/lib/`) にフィルタリング ロジックを追加または調整します。
- フィルタリングの動作をテストするテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

マージする前にテストを実行して合格する必要があります。Copilot Agent Mode にこれらのタスクを依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ レイヤー、UI、テストの各ファイルを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> プロジェクトの調査、ファイルの変更、テストの実行には数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業の完了後、すべての変更に **Keep** または **Undo** が表示されるため、進行中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したりテストが失敗したりすることがあります。Copilot はタスクを完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加しました。次のことを学びました。

- UI とデータ レイヤーにまたがる新機能を実装する方法。
- プロジェクトを調べ、関連ファイルを特定して変更を調整する方法。
- マージ前に変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### 追加の演習 - ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼します。既存の指示ファイルなどのベスト プラクティスに従い、ページ サイズをユーザーが選ぶか固定するかを検討し、希望する実装に必要なガイダンスをプロンプトに含めます。Copilot に変更やコンテキストを伝えて繰り返し改善することもできます。これは通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
