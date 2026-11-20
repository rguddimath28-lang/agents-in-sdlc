---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイして、誰でもアクセスできる場所に公開しましょう。Copilot はワークフローの準備とデプロイの案内を行いますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役割です。

このレッスンでは、次の内容を学びます。

- Copilot に GitHub Pages のデプロイを設定するよう依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの状況を追跡し、公開されたサイトを確認します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、誰でも簡単に試せる場所が必要です。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にデプロイを準備してもらう

> [!WARNING]
> プルリクエストの作成によって、コードがコミットおよびプッシュされる場合があります。このプロンプトを送信する前に、承認設定と送信先リポジトリを確認してください。**Approve all** を選択している場合、別途確認を求められないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. README が、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルドとデプロイ手順、トリガー、権限を含め、プルリクエストの変更をレビューします。
3. ワークフローが、マージされていないプルリクエストではなく、既定のブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新された差分をレビューします。マージを承認する前に、必須のプルリクエストチェックが成功するまで待ちます。

## マージとデプロイを承認する

> [!WARNING]
> 次のプロンプトを送信すると、レビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作に別途確認が求められない場合があります。

`<PR_NUMBER>` をレビューしたプルリクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで状況を確認します。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、タスクが完了したと見なさないでください。

## まとめと次のステップ

Space Quiz をサイトとして公開し、エージェントの報告だけに頼らず、デプロイを確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進んでください。

[next-lesson]: ../13-review/
