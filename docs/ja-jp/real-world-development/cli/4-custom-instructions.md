---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を作成したので、Copilot が使った命令を確認し、ドキュメントの標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの命令と、パスごとに適用される命令ファイルがエージェントに届く仕組みを確認します。
- コーディング標準が守られるように、命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの言い換えではなく、目的や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、挿入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述します。関連コードが変わったら、コメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使い、Copilot がこれらの開発慣例に沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コンテキストや希望事項を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、記述したいコメントの種類などを指定でき、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot が参照する単一の命令ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載するファイルです。

> [!NOTE]
> ほかの命令形式と対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム命令の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、あらかじめ命令ファイルが用意されています。変更を加える前に、既存ファイルを確認して、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクトファイルへのリンクを確認します。大きな命令を再利用可能な小さなファイルに分割し、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

すでにあるファイルも良い出発点ですが、TypeScript に新しく追加するコードに TSDoc コメントを記述するよう、`.github/copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. セクションの最後に、次の項目を追加します。

   ```markdown
   - 新しく作成する TypeScript には、ドキュメントとして TSDoc コメントを記述します。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの命令を読み込みます。更新した命令を適用するため、フィルタリングの会話を終了して再開します。機能のコンテキストはそのまま保持されます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   命令とコードのガイダンスを更新しました。このガイダンスに合うよう、生成済みのコードを更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用のカスタム命令を追加する][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
