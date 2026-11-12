---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、コンテキストを利用できるようにします。[指示ファイル][instruction-files]は、コード生成のガイダンスを伝える方法の1つです。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルが、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える仕組みを確認します。
- 現在の指示を使って、フィルタリング用データ スライスの最初の部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に沿う様子を確認します。
- 次の演習で Copilot が更新済みガイダンスを使えるよう、指示の更新とフィルタリングの作業を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示を更新した後の動作の「傾向」を確認します。生成結果が文字単位で一致する必要はありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データ レイヤーには必ず単体テストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件に関するコンテキストと希望を Copilot に伝えます。リポジトリ全体に適用するものと、特定のファイルやタスクに適用するものを作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリへのすべてのリクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、広く関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md`: TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加など、特定の対象にガイドラインを適用します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このプロジェクトの例は、指示ファイルの代表的な使い方を示しています。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、全体的なコーディング規約など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルにも AI にも、唯一の使い方はありません。プロジェクトに合った方法を見つけるために試行錯誤します。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを整備することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] などのタスク向けの指示があります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、利用する環境によって方法が異なります。該当するレッスンで説明します。
>
> テンプレートを探している場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトに含まれる指示ファイルを確認し、変更前の状態を見ておきます。`.github/copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** の指示も、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリ ルートからの glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. このプロジェクトの単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開きます。`unit-tests.instructions.md` など他の指示ファイルと、プロジェクト内の既存ファイルへのリンクがあります。パスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング規約がありますが、コード内のドキュメントはまだ必須ではありません。次に TSDoc コメントとファイル先頭コメントのルールを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストが変更されることがあります。
6. 生成されたヘルパーは、`db` クライアントを最初の引数に取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 生成コードには TSDoc コメントとファイル先頭コメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることがこの演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えます。コードの提案を改善するため、コーディング規約を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27行目付近の **Code formatting requirements** セクションを見つけます。コーディング規約は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の規約の直後に、ファイル先頭コメントと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、`.github/copilot-instructions.md` のようにリポジトリ レベルで作成して全体に適用するか、`*.instructions.md` のように特定の言語、ファイルの種類、タスクに適用できます。今回追加したドキュメント コメントのルールのように、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. ファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが付いていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、次の演習で利用する実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリ資産と同じようにソース管理します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎となる Copilot の変更をすべてステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示されたら、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチから PR を作成し、バックログのドキュメント標準に関する Issue に関連付けます。Copilot に Issue を探させ、Issue をクローズする PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を任せるのは今回が初めてです。プロジェクト テンプレートでは GitHub ツールがすでにワークスペースに接続されているため、セットアップは不要です。初回の利用時に VS Code から GitHub へのサインインを求められたら、案内に従って許可します。後の演習では、この接続 (Model Context Protocol) について説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、フィルタリング作業で使う publishers ヘルパーの基礎部分の両方を説明に含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を **Allow** するよう求められたら、コマンドを確認して適切な場合に許可します。
6. PR が作成されたら、次のプロンプトで PR のマージと `main` ブランチへの復帰を依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェル コマンドの実行を **Allow** するよう求められたら、内容を確認して適切な場合に許可します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に沿うことを確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基礎部分を `main` にコミットしました。

次は [agent mode][next-lesson]を使い、コードベース全体にまたがる新機能を Copilot に追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [カスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
