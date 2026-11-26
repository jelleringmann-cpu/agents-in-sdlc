---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、Copilot に焦点を絞った命令、ベストプラクティス、コーディング パターン、分野固有の知識を与え、特定の作業をより効果的に実行できます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントがあります。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述します。その後に、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを続けます。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最も分かりやすく区別するなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは専用のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエスト ワークフロー用や、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを行います。

- カスタムエージェントの定義方法を確認します。
- カスタムエージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を作成します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティのためのカスタムエージェントはすでに用意されています。内容を確認し、Copilot を導く方法を把握しましょう。Codespace に戻り、カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシビリティの高い Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

Mission Control は、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加のガイダンスを提供できます。高コントラスト モードを作成するタスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `ウェブサイトに高コントラストモードを追加する` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトに高コントラスト モードが必要です。ユーザーが高コントラストを切り替えるトグルを設置してください。設定はブラウザーのローカル ストレージに保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 利用可能なカスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![Copilot の割り当て画面で、カスタムエージェントと Accessibility が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取りかかります。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をより効果的に導けます。

このレッスンでは、次のことを確認しました。

- カスタムエージェントの定義方法
- カスタムエージェントにタスクを割り当てる方法

Copilot が高コントラスト モードを実装している間に、次は Mission Control から[エージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
