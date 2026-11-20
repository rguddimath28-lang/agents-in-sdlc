---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の作業を効果的に実行するための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に与えられます。チームの専門知識を再利用できるエージェントとしてまとめることができます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーに置く Markdown ファイルか、グローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

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

このレッスンでは、次の内容を学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot の作業をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ用カスタムエージェントを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシブルな Web サイトのコード生成に関する主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## issue を作成して割り当てる

ミッションコントロールは、環境内のすべてのエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、監視し、方向を変更したり追加のガイダンスを与えたりできます。まず、高コントラストモードを作成するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当て画面を開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの一覧で Accessibility agent が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門的な AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識と標準を再利用できる形でまとめ、特定の種類の作業をより効果的に進めるよう Copilot を導けます。

このレッスンでは、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot が高コントラストモードを実装している間に、次はミッションコントロールから[エージェントセッションを監視して方向付け][next-lesson]ます。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
