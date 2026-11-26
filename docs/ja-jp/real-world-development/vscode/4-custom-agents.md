---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、Copilot に焦点を絞った命令、ベストプラクティス、コーディング パターン、分野固有の知識を与え、特定の作業をより効果的に実行できます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントがあります。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述します。その後に、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを続けます。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最も分かりやすく区別するなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは専用のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエスト ワークフロー用や、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を作成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat のアクセシビリティ エージェントを使って高コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ作業のためのカスタムエージェントがすでに用意されています。内容を確認し、Copilot をどのように導くか把握しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します (演習 3 のフィルタリング PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシビリティの高い Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの命令とツールを使うチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** を選択します。すでに選択されている場合はそのままにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェント ドロップダウンを選択します。デフォルトでは、現在のエージェント (**default**) が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   サイトに高コントラスト モードを追加してください。ユーザーが高コントラストを切り替えられるトグルを設置し、ブラウザーのローカル ストレージを使ってページを再読み込みしても設定を維持してください。
   ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクトのファイルに適用します。各編集はファイル パスとレビューできる差分とともにチャットに表示されます。
6. 編集が追加されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリのファイルを実際に変更しながら、Astro コンポーネント、CSS、関連するテストなどを編集します。

次の演習では、進行中の作業をレビューし、方向付けを行います。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をより効果的に導けます。

このレッスンでは、次のことを確認しました。

- カスタムエージェントの定義方法
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法

次は[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライトモードのトグルを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
