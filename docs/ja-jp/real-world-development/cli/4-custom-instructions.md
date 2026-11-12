---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の基準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、希望するコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメント作成の基準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング規約に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- コメントには、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値、および該当する場合は注入可能な `db` 引数を説明する TSDoc/JSDoc を付けます。
- 再利用可能な Astro コンポーネントには `Props` の契約を説明するコメントを付け、関連コードが変わったらコメントも更新します。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に伝えられます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くの Copilot Chat または CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> 他の指示形式とそのサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには指示ファイルが含まれています。変更を加える前に、既存の内容とその影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加のルールが必要です。`copilot-instructions.md` を編集し、新しく生成する TypeScript に TSDoc コメントを付けるよう指示します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** というセクションを探します。
2. 末尾の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。フィルタリング機能のコンテキストを保ったまま、新しい指示を読み込むため会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認し、新しい基準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング規約に従うよう指示ファイルを更新しました。
- 指示ファイルの影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
