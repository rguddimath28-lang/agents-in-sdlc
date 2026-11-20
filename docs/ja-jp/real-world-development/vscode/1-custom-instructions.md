---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で行う必要がある場合や、Copilot が背景情報を知る必要がある場合は、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。コード生成で「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 既存の指示を使って、フィルター用の最初のデータスライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリング用のスライスを `main` にコミットし、次の演習で Copilot が更新済みの指針を使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から一部外れる場合があります。Copilot の動作は確率的です。この演習の目的は、指示の更新後に動作がどのような傾向で変化するかを確認することであり、出力を一字一句同じにすることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードを使い、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。関連性の高い提案やコードスニペットを得るために、Copilot の動作を調整する強力な機能です。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクのコンテキストにだけ適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送る多くのチャットや CLI のリクエストに関係する、プロジェクト全体の情報を記述します。たとえば、使用している技術スタック、開発中のものの概要、ベストプラクティス、全体に適用する指針などです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、新しい UI コンポーネントの作成、ユニットテストの追加といったタスクのガイドラインを記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[他の標準形式の指示ファイル][custom-instructions-support]もサポートしています。

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

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらは Copilot と行うすべての作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの glob を定義し、適用対象を決めます。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成の例として参照できます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントの規則を追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送ります。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。多くの場合、ヘルパーファイルとテストの両方を変更します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、出版社の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の改善が、この演習の要点です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コード提案を改善するために、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクト標準は記載されていますが、コード内のドキュメントはまだ規則になっていないため、先ほど生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル先頭のコメントと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル形式、タスク用の `*.instructions.md` ファイルとして作成することもできます。今追加したドキュメントコメント規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をクリックして、Copilot に対象ファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用の出版社データアクセスヘルパー。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての出版社の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 出版社オブジェクトの配列で解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用する実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリアセットと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (たとえばテスト) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、接続済みの GitHub ツールを使って Copilot に任せましょう。issue を見つけ、issue をクローズする PR を作成し、その後マージするよう依頼します。

> [!NOTE]
> Copilot が GitHub 上の操作を行うのは、この演習が初めてです。プロジェクトテンプレートには GitHub ツールがすでに設定されているため、追加設定は不要です。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。指示の変更と、新しい publishers ヘルパーの基礎の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作やシェルコマンドを許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映される様子を確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基礎を `main` にコミットしました。

次は[エージェントモード][next-lesson]で、コードベース全体にわたる新機能を Copilot に追加させ、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどを集めたリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
