---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。主な方法の 1 つが[命令ファイル][instruction-files]です。コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム命令と、パスごとに適用される命令ファイルを使って、プロジェクトのコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- 現在の命令を使って、フィルタリング機能の最初のデータ部分となるパブリッシャー ヘルパーを作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されるコードに新しい標準が反映される様子を確認します。
- 命令の更新とフィルタリング機能を `main` にコミットし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は決定論的ではありません。この演習の目的は、文字単位で同じ出力を得ることではなく、命令を更新した後の動作の変化を確認することです。

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

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot が使う単一の命令ファイルです。ほとんどの Copilot Chat または CLI リクエストに関係するプロジェクト全体のコンテキストを記載します。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別に関する命令を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスクに関するガイドラインを指定できます。

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
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクトの既存ファイルへのリンクがあります。大きな命令を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます (パスはリポジトリ ルートではなく、命令ファイルの場所からの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントに関するルールを追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令が適用された状態でプロンプトを送信します。その後、命令を更新してから同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. ヘルパーを配置する場所を Copilot に示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて名前と ID を返すデータ アクセス ヘルパーを追加してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを確認し、ヘルパー ファイルとテストなどに変更を適用します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイル先頭のコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。この演習では、命令の更新後に動作の一貫性が改善されることを確認します。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コード提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメントコメントが含まれていませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、ファイル先頭のコメントと TSDoc のドキュメントコメントを追加するよう Copilot に指示できます。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの目的を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルのファイル (`.github/copilot-instructions.md`) は全体的なガイダンスに、`*.instructions.md` ファイルは特定の言語、ファイル種別、タスクに関する命令に使えます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話コンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルを対象にするようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて名前と ID を返すデータ アクセス ヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム向けのパブリッシャー データ アクセス ヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns パブリッシャー オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準を追加し、Copilot を導いて、次の演習で使う実際のコードに適用しました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもリポジトリ内のほかのアセットと同じようにバージョン管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace で新しいターミナルを開きます。
2. 次のコマンドを実行し、命令の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで命令ファイルの更新、ヘルパーの基盤、Copilot が変更した関連ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って、Copilot に issue を探し、PR を作成して、その後マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 操作を依頼します。プロジェクト テンプレートには GitHub ツールがすでに接続されているため、追加のセットアップは不要です。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従ってください。この接続の仕組み (Model Context Protocol) は、後の演習で確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 命令ファイルの更新と、今後のフィルタリング作業に使うパブリッシャー ヘルパーの基盤を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```
   命令ファイルの更新に関する issue を見つけてください。現在のブランチから新しい PR を作成し、その issue を解決することを説明してください。今後のフィルタリング作業に向けて、パブリッシャー ヘルパーの基盤も追加したことを含めてください。
   ```

4. Copilot が issue を探して PR を作成する様子を確認します。
5. Copilot に GitHub 操作を許可するよう **Allow** を求められたら、コマンドを確認して、適切な場合に許可します。
6. PR が作成されたら、PR をマージしてローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新の状態にしてください。
   ```

7. Copilot に GitHub 操作とシェル コマンドを許可するよう **Allow** を求められたら、コマンドを確認して、適切な場合に許可します。

GitHub Copilot の助けを借りて、プルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の命令が適用された状態でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認しました。
- 命令の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で命令を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のより良いカスタム命令を作成する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
