---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価するため、品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR をマージしました。

1. **星評価**: 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR を確認しました。

追加するテストは、実際の不足を補うものでなければなりません。QA を実行した結果、新しいテストが不要な場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は関連する根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の内容を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は目的を絞った変更を自分で確認しながら進めるとき、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいのかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` で、コードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について対話し、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを分離します。
- `/delegate` で、タスクを Copilot cloud agent に送信します。

## 次のステップ

どんなツールも、使い続けることで上達します。実際のコード、趣味のコード、何年も作りたいと思いながら手をつけていなかった小さなアプリなどで活用してください。学んだことをチームで共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
