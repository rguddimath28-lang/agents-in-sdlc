---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の作業を効果的に実行するための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に与えられます。チームの専門知識を再利用可能なエージェントとしてまとめることができます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーに置く Markdown ファイルで定義します。また、`~/.copilot/agents` にグローバルなエージェントを定義できます。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。わかりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを統括します。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルやテストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントに唯一の正しい作成方法はありません。AI に関する他のことと同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングのプラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいるという意見が寄せられました。このアクセシビリティの課題に対処するため、デザインチームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次の内容を学びます。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot の作業をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します (演習 3 のフィルター PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシブルな Web サイトのコード生成に関する主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` で定義されたすべてのカスタムエージェントが Copilot Chat ビュー下部のエージェントのドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせたチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントのドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送ります。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャットに表示され、確認できます。
6. エージェントが編集したファイルの一覧は、**Files changed** の表示に反映されます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリに実際の変更を行います。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習で、進行中の作業をレビューし、方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門的な AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識と標準を再利用できる形でまとめ、特定の種類の作業をより効果的に進めるよう Copilot を導けます。

このレッスンでは、次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]ます。変更を確認し、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
