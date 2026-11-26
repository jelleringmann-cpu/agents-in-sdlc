---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は、機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質ワークフローに関する作業を同じ会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能を開発する中で、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなど) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したら、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot が「PR #14 を squash merge しました」のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発作業の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、省略可能なクラウドへの委任に使える[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
