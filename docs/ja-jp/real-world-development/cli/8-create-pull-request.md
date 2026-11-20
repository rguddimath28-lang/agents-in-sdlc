---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。ワークショップを効率よく進めるため、フィルターと品質ワークフロー全体を 1 つの会話とブランチで進め、すべての作業をこの PR にまとめました。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルターの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルターのワークフロー全体で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの許可を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストのマージに必要な残りの作業を自動化します。有効にすると、失敗した継続的インテグレーション (CI) のチェックを修正し、レビューコメントに対応し、必要に応じてリベースするなど、PR を妨げている問題に Copilot が対処します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、準備ができていることを確認してください。

## Agent Merge に PR を管理させる

コードの作成が終わったら、すべてをレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot が「PR #14 was squash-merged successfully.」のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルターの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
