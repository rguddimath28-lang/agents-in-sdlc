---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

Cloud agent に割り当てたタスクを管理するツールはいくつかあります。GitHub.com の[エージェントページ][agents-page]もその 1 つです。このミッションコントロールでは、オープンなプルリクエスト (PR) があるエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示して作業の方向を調整したりできます。

このレッスンでは、次の内容を学びます。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示し、追加機能を依頼します。

## シナリオ

高コントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能をすでに作業しているため、一緒に追加するのが合理的です。高コントラストに加えてライトモードも実装するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。Copilot に割り当てた次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためのさまざまな手順を実行する。

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、監視対象の PR と変更ファイルを確認できます。

## Cloud agent に指示する

実行中のタスクを確認したので、エージェントが高コントラストモードに取り組んでいる間に、ライトモードの切り替えも追加するよう依頼します。

1. 高コントラストモードを追加するセッションを選択します。エージェントが付けた名前や作業状況によって、タイトルは異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の cloud agent に指示するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に加えたことを確認します。

## Copilot に作業を続けてもらう

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を完了した後、新しい依頼を作業の流れに取り込みます。

作業には数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot agents ページを使って GitHub Copilot cloud agent のタスクを監視し、指示しました。このミッションコントロールでは、実行中と完了済みのタスクを追跡し、進行中の作業を確認して、範囲の変更や追加のガイダンスを行えます。

このレッスンでは、次の内容を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示し、追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は Copilot が作成したプルリクエストを[反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
