---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験と機能の改善を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが改善を検討できる関連ゲーム機能を作り、高コントラストとライトモードのトグルを含むアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って作業を改善します。

### シナリオ

これまでに何度も説明したように、生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進める必要があります。チームのレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を保つ目的でいくつかの制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、コードの作業に使うブランチに限って書き込みアクセスが許可されます。
- Cloud agent は GitHub Actions 内に、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行する前に、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がコードにドキュメントを追加した最初のプルリクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際に、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。MCP サーバーへのアクセスなど、外部リソースへのアクセスはデフォルトで制限されているため、これは想定どおりの動作です。必要であれば、[Copilot cloud agent のファイアウォールを設定または無効化できます][agent-firewall]。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **不足しているドキュメントを追加**またはそれに近いタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメントコメントや、その他のドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。

   変更を確認しながら、TSDoc のドキュメントコメント、TypeScript の規約、ファイル先頭のコメントを確認してください。これらは演習 2 の最初に確認したカスタム命令ファイルの内容に基づいています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と一方向にやり取りするだけではありません。チーム メンバーに依頼するように、PR コメントやコードのインライン コメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は一意ではないため、必ず成功する特定の文面を指定することはできません。Copilot に依頼できる変更の例は次のとおりです。

- 各コード ファイルの先頭に、ファイルの役割を短く説明するコメントを追加します。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. 上記の例のいずれか、またはコードベースのドキュメントに関する別の提案を使い、**@copilot** をタグ付けして、生成されたドキュメントの変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

納得できる結果になるまで、繰り返し作業できます。満足したら、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージします。

![レビュー可能な状態に変更する PR](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. タイトルが **ゲーム詳細ページに関連ゲームを表示する**、またはそれに近い PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。これまでと同じように、Copilot cloud agent に更新を依頼して、繰り返し改善できます。たとえば、表示する関連ゲームの数の変更や、カスタム命令を更新する**前**に割り当てたタスクのため、ファイル先頭のコメントと TSDoc のドキュメントコメントを追加するよう依頼できます。前と同じく、**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 の Mission Control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. タイトルが **ウェブサイトに高コントラストモードを追加する**、またはそれに近い PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI トグル
   - ユーザー設定を維持するローカル ストレージの使用
   - 高コントラスト モードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラスト モードとライトモードのトグルを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティ カスタムエージェントによって、Copilot はアクセシビリティのベストプラクティスに従って機能を実装できました。アクセシビリティについて気になる点や改善案があれば、前の PR と同様にコメントで **@copilot** をタグ付けし、更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent には、実際のバックログ項目を任せ、自分はほかの作業に集中できます。この方法に慣れるため、リポジトリに issue をいくつか追加し、Copilot に割り当ててみましょう。次のような作業を試せます。

- ゲーム詳細ページに支援者向けの関心登録フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データ アクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに用意されたカスタム命令ファイルを確認しました。** 後で cloud agent の結果に反映されている内容を確認できます。
- **issue を Copilot cloud agent に割り当てました。** 環境を設定し、非同期に計画・実行する様子を確認しました。
- **カスタムエージェントを作成して使いました。** アクセシビリティ エージェントで、高コントラストとライトモードのトグルを追加しました。
- **Copilot agents ページを Mission Control として使いました。** アクセシビリティ セッションを監視し、作業中に方向付けしました。
- **cloud agent のプルリクエストをレビューし、改善しました。** `@copilot` をタグ付けして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容を別の環境で試します。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリで、次のようなアイデアを試すこともできます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
