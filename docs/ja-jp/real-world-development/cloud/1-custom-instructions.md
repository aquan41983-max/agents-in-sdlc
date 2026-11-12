---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、コンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルで、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習では変更前後を比較するプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、影響をリアルタイムに並べて示すのは困難です。cloud agent が生成するプルリクエストをレビューする後の演習で、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データ レイヤーには必ず単体テストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件に関するコンテキストを Copilot に伝えます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、プロジェクト概要、ベスト プラクティスなど、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro など特定の言語、UI コンポーネントの作成や単体テストなどのタスクに関する指示を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

スターター プロジェクトの例は、指示ファイルの代表的な使い方を示しています。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、全体的なコーディング規約など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関するガイドラインを記載します。
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

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを読み、Copilot が受け取るコンテキストを確認します。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** の指示は、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリ ルートからの glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストに適用されます。
5. このプロジェクトの単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開きます。`unit-tests.instructions.md` など、他の指示ファイルやプロジェクト ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、例を示せます。ここでのパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約がありますが、コード内のドキュメントはまだ必須ではありません。次に TSDoc コメントとファイル先頭コメントのルールを追加します。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えます。コードの提案を改善するため、コーディング規約を明記しましょう。

変更前に作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更をコミットするためにブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング規約が記載されていますが、コード内のドキュメントに関するルールがありません。
4. 既存の標準の直後に、ファイル先頭コメントと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` のどちらでも作成できます。今回のドキュメント コメント規則のようなプロジェクト標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が更新した指示を使えるよう、変更を `main` にマージします。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこれらの指示を読み取ります。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する仕組みを確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューする際に、この標準が使われます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
