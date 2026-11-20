---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の作業フローに沿って、適切なものを適切な方法で構築できるように設計されています。このレッスンから、エージェントを使った一般的な開発プロセスをいくつかのレッスンに分けて進めます。issue から新機能を作り、コードの妥当性と機能の動作を確認し、最後にプロジェクトへマージします。

> [!NOTE]
> 機能の作業中は、同じ会話とブランチを使います。通常、ファイルの種類ごとに別のブランチや PR を作りますが、主要な概念に集中するため、ここでは手順を簡略化します。

このレッスンでは、次の内容を学びます。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- ポート転送したブラウザーで機能を手動で検証します。

引き続き機能の作業を進めながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや出版社でゲームを絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意する詳細があります。Plan モードで方針を決めてから、Autopilot に範囲を明確にした実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を記述した issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れに沿っています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集は禁止されます。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復して自律的に作業します。

まず Plan モードで計画を作り、レビューしてから Autopilot で実装します。

## issue から始める

フィルター作業を始める前に Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画するのが効果的です。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に沿って答えます。正解は 1 つではありません。
4. 計画が完成したら <kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 決めた方針と一致しない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot モードで Copilot に作業を任せます。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンにある同様の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. issue と計画時の決定に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を確認する

コードを確認できました。次に、アプリを起動し、Codespaces でポート転送されたサイトをブラウザーで開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で合意したカテゴリと出版社のフィルター、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が issue と決定した内容に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の内容を学びました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- ポート転送したブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]コードの生成方法をさらに詳しく確認し、文書化された開発手法に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
