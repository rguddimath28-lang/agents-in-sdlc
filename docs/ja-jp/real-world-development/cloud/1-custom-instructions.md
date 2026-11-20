---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で行う必要がある場合や、Copilot が背景情報を知る必要がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使ってガイダンスを伝え、Copilot が「何を」するかだけでなく、「どのように」行うかも理解できるようにします。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では更新前と更新後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードを使い、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。関連性の高い提案やコードスニペットを得るために、Copilot の動作を調整する機能です。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクにだけ適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送る多くのチャットや CLI のリクエストに関係する、プロジェクトレベルの情報を記述します。たとえば、使用している技術スタック、開発中のものの概要、ベストプラクティス、全体に適用するガイダンスなどです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、新しい UI コンポーネントの作成、ユニットテストの追加といったタスクのガイドラインを記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、[他の標準形式の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトの例は一般的な方針を示しています。主なポイントは次のとおりです。

- `copilot-instructions.md` には、開発しているもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルの指針を記述します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や特定のタスクに関する指示を記述します。
- 自然な言葉で、明確なガイドラインを書きます。コードの良い例と悪い例を示します。

AI の使い方に唯一の正解がないのと同じように、指示ファイルの作り方にも唯一の方法はありません。プロジェクトに合う方法は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトに含まれるファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示があることがわかります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境で方法は異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このレッスンでは、必要な操作を該当する箇所で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決めるリポジトリルート相対の glob を指定しています。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成の例を Copilot に示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントの規則を追加します。

## リポジトリ全体の標準を追加する

次は、コード内のドキュメントを TSDoc のドキュメントコメントとファイルレベルのコメントで記述するプロジェクト全体の規則を追加します。このハーネスの後半で issue を割り当てると、cloud agent がこの規則に従います。

編集する前に、作業用のブランチを作成します。Cloud agent は issue の対象ブランチから指示を読みますが、変更を適切にコミットするため、ここではブランチを作成します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コード提案を改善するために、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関する規則がまだありません。そのため、先ほど生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル先頭のコメントと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル形式、タスク用の `*.instructions.md` ファイルとして作成することもできます。今追加したドキュメントコメント規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は issue が対象とするブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使えるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で割り当てる issue に取り組むとき、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトでの作業方法を再現するためにブランチを作成します。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が生成するプルリクエストに、この標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどを集めたリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
