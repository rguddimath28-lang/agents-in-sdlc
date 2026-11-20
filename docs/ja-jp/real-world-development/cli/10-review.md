---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

一連の Tailspin Toys ワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、仕込まれていたフィルターの issue を見つけました。
- 星評価を追加し、ポートフォワードしたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの issue に取り組み、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーの証拠を評価しました。
- フィルターの変更全体をレビューし、フィルターの PR に対する Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 提出した成果

このワークショップには、更新済みの `main` からそれぞれブランチを作成して進める、2 つの PR の節目があります。

1. **星評価:** ゲームカードに、既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新してその機能に適用し、`quality-checks` レポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは、PR を作成する前に要件とテスト範囲のレビューを組み合わせました。

テストを追加するのは、本当に存在する不足を解消するときです。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果をレビューしてください。変更があった場合は、影響を受ける検証をもう一度実施します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズの変化に合わせて調整してください。指示は標準を定め、スキルは繰り返し行うタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築の前に方針を考えるには **Plan**、範囲を絞った変更に取り組みながら作業を管理するには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に伝えると、出力は大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、ほかにも試す価値のある機能があります。

- `/review` を使って、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` を使って問題を整理し、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並列に進めます。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。製品コード、趣味のコード、何年も作ろうと思いながら手を付けられなかった小さなアプリなどで活用してください。学んだことをチームで共有し、チームのメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境を試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
