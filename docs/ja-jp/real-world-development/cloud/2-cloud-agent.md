---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるようにドラフト PR を作成します。面倒な作業を任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを行います。

- コード生成環境をカスタマイズします。
- 操作が安全に行われるようにします。
- 範囲を明確にした issue の重要性を確認します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを十分に作成していませんでした。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを「関連ゲーム」として表示したいという要望です。完成度の高い実装はまだ必要なく、ユーザー体験 (UX) の受け入れテストに使える機能があれば十分です。現在は、より優先度の高い issue があるため、この作業は後回しになっています。

どちらも優先度が下がりやすいタスクであり、Copilot cloud agent に任せるのに適しています。Copilot cloud agent が非同期で作業している間、開発者はほかのタスクに集中できます。後で戻って、Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などで作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような機能はありません。考える過程を省略することはできません。一見簡単そうな作業も、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot を導く仕組みを確認しました。カスタム命令の演習をすでに行った場合は、`.github/copilot-instructions.md` を使ったことがあるでしょう。まだ確認していない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意されている命令ファイルを読んでください。後で影響を確認するため、ここでは読むだけにします。

GitHub Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - TypeScript の規約と TSDoc ドキュメントコメントの要件が記載された **Code standards** セクションを確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter が `**/*.test.ts` ファイルに命令を適用する方法を確認します。

次のセクションで「コードにドキュメントがありません」という issue を cloud agent に割り当てると、作成されたプルリクエストに TSDoc ドキュメントコメント、TypeScript の規約、ファイル先頭のコメントが反映されているか確認できます。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は適切な環境を整え、状態を確認するためのセットアップ スクリプトを実行します。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルで[特別なセットアップ ワークフロー][setup-workflow]を設定すると、作業を始める前に必要な開発ツールと依存関係を準備できます。このワークショップでは、学習をスムーズに進められるよう、設定をあらかじめ用意しています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 各手順で必要となる最低限の権限を設定します。Copilot は操作に専用のトークンを使用します。
    permissions:
      # 依存関係をインストールするなど、セットアップ手順でリポジトリをクローンする場合は contents: read 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンドとアプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと似ていますが、いくつかの重要な特徴があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成にも役立つため、Copilot cloud agent に任せるのに適したタスクです。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `コードにドキュメントを追加する` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織の要件として、関数やメソッドには役立つ場合に TSDoc のドキュメントコメントを含める必要があります。しかし、最近の更新ではこの標準が守られていません。動作を明確にするドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: コードにドキュメントを追加する` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行します)、プロジェクトを分析して、課題に取り組む方法を検討しています。

13. todo リストと、実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot がどのように課題を解決しているかを確認します。コードを調べて状態を把握し、計画を考えて、コードを作成する様子を確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、非同期にタスクを実行できることです。別の作業に取り組めるよう、この機能を使って別のタスクも Copilot cloud agent に割り当ててから、アプリケーションに機能を追加します。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。自分があるタスクに取り組んでいる間に、Copilot に別のタスクを任せられます。デザイン チームのために関連ゲームのセクションを追加する作業が短時間で終わるとしても、その時間をほかのタスクに使えます。この作業を Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    各ゲームの詳細ページに関連ゲームを表示して、ゲームを見つけやすくしたいと考えています。デザイン チームはユーザー体験を確認し、受け入れテストを行う予定です。要件は次のとおりです。

    - `src/lib/` に、指定したゲームを除く同じカテゴリのほかのゲームを返すデータ アクセス ヘルパーを追加する
    - ヘルパーを使う「関連ゲーム」セクションをゲーム詳細ページに表示する
    - 関連ゲームがない場合を処理する
    - 新しいヘルパーのユニットテストを作成する
    - PR を作成する前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

少しすると、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![目の絵文字で issue に取り組んでいることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、作業中ずっと監視する必要はありません。完了したら結果をレビューします。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期で作業を実行できます。Copilot に技術的負債の解消、新しい機能の作成、フレームワーク間のコード移行などを依頼できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする方法
- 操作が安全に行われるようにする方法
- 範囲を明確にした issue の重要性
- issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も利用でき、前のモジュールで確認したカスタム命令も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
