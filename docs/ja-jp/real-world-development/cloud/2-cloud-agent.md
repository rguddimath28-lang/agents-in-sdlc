---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能の依頼などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業を終えると、レビュー担当の開発者が確認できるドラフト PR を作成します。面倒なタスクを任せることで、開発を加速し、より大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成のための環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初の Web サイトを作成した委託先がドキュメントを適切に整備していません。実際、ドキュメントがまったくありません。最初に、アプリケーションのすべてのエクスポート関数へ TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、もっと優先度の高い issue もあります。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に割り当てるのに適しています。Cloud agent が非同期で作業している間、開発者は別のタスクに集中し、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]られます。割り当てると Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を学びました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、cloud agent に作業を依頼する前に、このリポジトリに用意された指示ファイルを確認しましょう。

GitHub Web UI で次のファイルを開くか、Codespace をすでに起動している場合はそこで開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、生成された PR に TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているか確認できます。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent 用の開発環境をセットアップする

コードの作成には、通常、特定の環境と初期セットアップが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業するからです。

Cloud agent は GitHub Actions を使って作業環境を構築します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係をインストールできます。ラボを円滑に進めるため、この環境は事前に設定されています。単一の Astro アプリのために Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションと初期データ投入済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot には操作用の独自トークンが渡されます。
    permissions:
      # 依存関係をインストールするなど、セットアップ手順の一部としてリポジトリをクローンする場合は contents: read 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
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

      # ローカルの SQLite データベースをマイグレーションして初期データを投入し、ビルドとテスト用データを準備します。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは他の GitHub ワークフローファイルとよく似ています。主なポイントは次のとおりです。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブから手動で実行できます。Copilot が実行するまで待たずにワークフローをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古い、またはドキュメントがないプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当て画面を開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、1、2 分待ってブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を構築し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断した後、コードを作成する様子を確認できます。

数分かかる可能性があります。Copilot cloud agent の主な目的は、非同期にタスクを行い、ユーザーが他の作業に集中できるようにすることです。この特徴を活用し、別のタスクも cloud agent に割り当ててから、アプリケーションの機能を追加するコードに取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームが求めている関連ゲームの表示は長時間かからないかもしれませんが、その時間を他の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当て画面を開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後すぐに、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![目の絵文字で issue の作業中を示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するので、常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]について学びました。Cloud agent に issue を割り当て、非同期に作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

このレッスンでは、次の内容を学びました。

- コード生成のための環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示も使います。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

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
