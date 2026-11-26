---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使って、ブラウザーでフィルタリング機能を手動でテストしてもらいます。
- ブランチを公開し、フィルタリングの作業に関するプルリクエストを開きます。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに強力になります。Copilot がそのために使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わり操作を実行したりできます。

これらのツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。既存の API や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントが新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントが Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースにアクセスできる、ほかの MCP サーバーも多数あります。GitHub は、エコシステム内の MCP を見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはほかの依存関係と同じように扱ってください。使用する前にソースコードをよく確認し、公開元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密情報や重要な操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ウィンドウを表示せずにブラウザーを実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使っているため、必要なブラウザーもすでにインストールされているはずです。Copilot が後でブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は、Copilot がサーバーのツールを初めて必要としたときに MCP サーバーを**オンデマンドで**起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するよう求められた場合は、設定を確認し、サーバーの起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを使う際に VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開き、移動し、フィルターを適用し、結果を読み取って要約してもらえます。会話を中断せず、機能が期待どおりに動くことをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を基に処理します。これは支援技術と同じ方法です。そのため、簡単な機能確認をアクセシビリティの基本チェックとしても活用できます。

サーバーを接続し、アプリを実行した状態で、Copilot に作成したフィルタリング機能を試してもらいます。

```text
Playwright MCP サーバーを使って、http://localhost:4321 で実行中のアプリをブラウザーで開き、新しいゲームのフィルタリング機能を確認してください。

1. ゲームのページに移動し、ゲームが何件表示されているか確認します。
2. カテゴリ フィルターを適用し、一覧がそのカテゴリのゲームだけに更新されることを確認します。
3. フィルターを解除し、パブリッシャー フィルターを適用して、そのパブリッシャーのゲームだけに更新されることを確認します。
4. カテゴリとパブリッシャーの両方を指定し、両方の条件に沿った結果になることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して、結果を報告します。issue の受け入れ条件に照らして報告を確認してください。問題があれば、追加の質問をするか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使う際、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームでレビューするためにプルリクエスト (PR) を開きましょう。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot の変更を確認します。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを使って、コミット メッセージを生成します。

   ![変更内容を表示した Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。関連する issue を探し、関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue に関連付けてください。
   ```

4. Copilot が情報を集めたり操作したりするため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。プロンプトに従って認証します。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択し、プルリクエストをレビューします。ただし、まだマージ**しないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーでフィルタリング機能を手動でテストしてもらいました。
- ブランチを公開し、フィルタリング作業のプルリクエストを開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つに過ぎません。次は、アクセシビリティのレビューのような目的を絞ったタスクを効率化するため、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
