---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。その 1 つが GitHub.com の[エージェント ページ][agents-page]です。この Mission Control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。エージェントの操作を調べたり、実行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- cloud agent のタスクを監視するために、エージェント ページを確認します。
- 進行中のセッションに指示を追加し、機能を拡張します。

## シナリオ

エージェントに高コントラスト モードの作成を依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが自然です。エージェントの作業を方向付け、高コントラストに加えてライトモードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。Copilot に割り当てたタスクとして、次の内容が表示されます。
   - コードのドキュメントを更新する
   - ゲーム変更用の API を生成する
   - Web サイトに高コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した操作を確認します。次のような操作があります。
   - リポジトリのコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認できたら、高コントラスト モードに取り組んでいる Copilot にライトモードのトグルも追加するよう依頼します。

1. 高コントラスト モードを追加しているセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![Mission Control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを完了して作業を始めるまで、数分待ちます。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   高コントラスト モードを作成するのに合わせて、ライトモードも追加してください。ユーザーが表示モードを選べるよう、ライトモード用の切り替えも設けてください。
   ```

   ![Agents ページで、作業中の Copilot に指示を追加するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に含めたことを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、Copilot は更新されたタスクに取りかかります。プロンプトを送信した時点で実行していた手順が終わると、新しい依頼を作業の流れに取り込みます。

この作業にも数分かかります。待っている間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページで、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所を確認しました。Mission Control では、実行中や完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクの方向を変更できます。

このレッスンでは、次のことを確認しました。

- Mission Control とエージェント ページで、cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加して、機能を拡張しました。

Copilot がアクセシビリティ機能の作業を続ける間に、次は[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
