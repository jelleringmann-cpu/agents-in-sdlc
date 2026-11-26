---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot にガイダンスを伝え、何をしてほしいかだけでなく、どのように実行してほしいかも理解してもらえます。

この演習では、次のことを行います。

- リポジトリのカスタム命令と、パスごとに適用される命令ファイルを使って、プロジェクトのコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後を比較するプロンプトを実行しません。Copilot cloud agent は GitHub issue に対して非同期に作業するため、リアルタイムで並べて効果を示すのは困難です。cloud agent が作成したプルリクエストをこのハーネスの後半でレビューする際に、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

命令ファイルを使い、Copilot がこれらの開発慣例に沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コンテキストや希望事項を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、記述したいコメントの種類などを指定できます。リポジトリ全体、または特定のファイル種別に適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot が使う単一の命令ファイルです。多くの Copilot Chat または CLI リクエストに関係するプロジェクト全体のコンテキストや情報を記載します。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別に適用できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスクに関するガイドラインを記載します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コードの生成時にリポジトリ レベルの命令と `applyTo` frontmatter がある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイルを管理するためのベストプラクティス

命令ファイルの作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプル プロジェクトには一般的な方法を示す例が含まれています。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する命令を記載します。
- 自然な言葉を使い、ガイダンスを明確にします。コードをどのように記述するか、またどのような書き方を避けるか、例を示します。

命令ファイルの作成方法や AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、十分に整備された命令ファイルを用意しましょう。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに関する命令があります。
>
> Copilot に命令ファイルの作成を依頼することもできます。環境によって方法は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。各環境向けのレッスンで、関連する方法を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` ファイルが 1 つと、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらの命令は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます (パスはリポジトリ ルートではなく、命令ファイルの場所からの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントに関するルールを追加します。

## リポジトリ全体に適用する標準を追加する

次はこの演習で行う唯一の編集作業です。コードに TSDoc のドキュメントコメントとファイル先頭のコメントを追加するよう、プロジェクト全体のルールを設定します。後で issue を Copilot cloud agent に割り当てると、この命令が使われます。

編集の前に作業ブランチを作成します (cloud agent は issue の対象ブランチにある命令を読み取りますが、変更をきちんとコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コード提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメントコメントが含まれていませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、ファイル先頭のコメントと TSDoc のドキュメントコメントを追加するよう Copilot に指示できます。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの目的を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルのファイル (`.github/copilot-instructions.md`) は全体的なガイダンスに、`*.instructions.md` ファイルは特定の言語、ファイル種別、タスクに関する命令に使えます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は、issue が対象としているブランチの命令ファイルを読み取ります。次の演習では Copilot に issue を割り当て、`main` からブランチを作成します。cloud agent が命令を参照できるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue を Copilot に割り当てると、cloud agent はこの命令を使います。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成を省略して、そのままコミットしてもかまいません。実際のプロジェクトでの作業方法を示すため、ブランチの手順を含めています。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りで cloud agent が作成するプルリクエストをレビューすると、標準が適用されていることを確認できます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のより良いカスタム命令を作成する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
