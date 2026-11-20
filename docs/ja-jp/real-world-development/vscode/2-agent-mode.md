---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストを実行するなど、さまざまな作業が必要です。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけ、さらに更新する、といった作業を繰り返します。作業は際限なく続くことがあります。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、まず既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに作られています。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りするペアプログラマーとして設計されています。

この演習では、次の内容を学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや出版社で絞り込めるようにしたいと考えています。前の演習で publishers ヘルパーを追加しました。次は Copilot エージェントモードを使って、データレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧表示と詳細表示ができます。サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小さなスコープ付き Astro `<script>` を使います。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば `main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。その際、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押したままにします。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace へのプライベートトンネルであり、Codespace が Web サーバーをホストしています。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、出版社、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

最初の Web サイトは動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、issue のバックログも自動的に作成されています。GitHub Copilot にそれらの項目を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** はファイル間の変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案されるコードに一貫性を持たせ、よい体験を提供することを意図しています。ただし、LLM は確率的に動作するため、ワークショップに記載されたものと異なる提案が出ることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考える項目を優先順位付けしています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認しましょう。ここでは、Web サイトに機能を追加する方法を示す [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の方針に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - 次のファイル形式に関する指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、例やテンプレートを提示できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルター処理を追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページを更新して、フィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。これらの作業を Copilot エージェントモードに任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat パネルでテストの実行許可を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

お疲れさまでした。この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の内容を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装させましょう。次の点を検討します。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、値を固定するかなど、ページネーションの方法を検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に与えます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
