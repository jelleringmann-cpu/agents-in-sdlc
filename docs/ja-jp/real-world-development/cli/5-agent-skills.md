---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、命令ファイルを使ってコーディング標準に従うようにしました。次はテストや lint、継続的インテグレーション (CI) のほかの作業について確認します。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- レポートの形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む再利用可能なタスク命令、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキル名と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスが含まれます。スキルで使用するスクリプトやリソースをサブフォルダーに追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するためにプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、テストと lint の手順をどのように案内しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI からスキルを直接呼び出すか、自然な言葉で依頼できます。スキルに 3 つのチェックを実行してもらいましょう。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容が分かるレポートに改善しましょう。スキルを更新して、必要な情報を出力するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に、次のセクションを追加します。

   ```markdown
   ## 結果の出力形式

   完了後、実行したコマンドごとに、成功、失敗、またはブロックされたかを報告します。ツールが報告した場合に限り、テスト数、所要時間、エラー、警告、その他の指標を含めます。失敗やブロックがある場合は次に行う作業を示し、スキップされたチェックや未完了のチェックを成功として報告しないでください。
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行して動作を確認します。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR でフィルタリングの実装と一緒に扱います。次は [Playwright MCP サーバー][next-lesson]を使い、Copilot にサイトを直接操作してもらいます。

## その他のスキル例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と組み込みのエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
