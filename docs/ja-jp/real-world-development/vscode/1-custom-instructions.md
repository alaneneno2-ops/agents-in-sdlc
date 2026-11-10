---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝える必要がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、コード生成の指針を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 既存の指示を使って、絞り込み機能の最初のデータ部分となる publishers ヘルパーを生成します。
- `.github/copilot-instructions.md` にリポジトリ共通の標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新と絞り込み機能の基盤を `main` にコミットし、次の演習で新しい指針を使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確定的ではありません。この演習では、更新後の動作傾向を確認します。出力が文字単位で同じになることを期待しないでください。

## 指示ファイル

### シナリオ

Tailspin Toys では、次の開発ガイドラインを定めています。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコードを生成できます。リポジトリ全体、または特定のファイルやタスクを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベスト プラクティスなど、プロジェクト全体に関係する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式を対象にした指針です。TypeScript や Astro などの言語や、UI コンポーネントやユニット テストの作成などのタスクに利用できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。詳細は[カスタム指示のサポート状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく扱うことはこの演習の範囲外ですが、サンプル プロジェクトには代表的な構成例があります。基本的には、次の点を意識します。

- `copilot-instructions.md` には、プロジェクトの概要、構成、全体のコーディング標準など、プロジェクト レベルの情報を記述します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル形式や、特定のタスク向けに使います。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作成方法も、AI の使い方と同様に一通りではありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットを用意することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境ごとに操作方法が異なります。必要な場合は、各ハーネスの手順で説明します。
>
> テンプレートや出発点を探すには、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクト概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリのルートからの glob で、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内のファイルへのリンクを確認します。相対パスは指示ファイルの場所を基準にしています。指示を小さく再利用可能なファイルに分け、コード生成の参考例を示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクト標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。次の手順で TSDoc コメントとファイル先頭のコメント ブロックを追加します。

## 変更用ブランチを作成する

変更をブランチで管理するため、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な手順は各所で説明します。

指示を更新する前にプロンプトを送信し、現在の指示がある場合に生成される内容を確認します。後で指示を更新して、同じプロンプトをもう一度送信します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に渡すコンテキストを整理します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新する様子を確認します。ヘルパー ファイルとテストを更新することがあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案コードに TSDoc ドキュメント コメントとファイル先頭のコメント ブロックが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確定的に動作しないため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ共通の標準を追加する

`.github/copilot-instructions.md` にコーディング標準を記述し、コード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準は記載されていますが、コード内のドキュメントに関する規則はありません。
3. ファイル先頭のコメントと TSDoc コメントを追加するため、既存の標準の直後に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体に適用する標準は `.github/copilot-instructions.md` に、言語やファイル形式、タスク固有の指示は `*.instructions.md` に記述します。今回追加したドキュメント コメントの規則はプロジェクト全体に適用するため、リポジトリ レベルのファイルが適しています。

## 同じプロンプトを再実行して変化を確認する

指示を更新した後、同じプロンプトを使い、生成コードに変化があるか確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームの発行元データアクセス ヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * すべての発行元の ID と名前を含む一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリ ファイルと同じようにソース管理します。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. `git status` を実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤を作るために Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているので、Copilot に Issue を探し、PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、これが最初です。プロジェクト テンプレートでは GitHub ツールがあらかじめ設定されていますが、Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められることがあります。プロンプトに従って認証します。この接続の仕組み (Model Context Protocol) は後の演習で扱います。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新と発行元ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成するのを待ちます。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認して、適切な操作のみ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行を許可するよう求められた場合は、コマンドを確認して、適切な操作のみ **Allow** を選択します。

GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ共通の標準を追加しました。
- 同じプロンプトを再実行し、提案コードに標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[agent mode で指示を活用し][next-lesson]、コードベース全体にまたがる機能を Copilot に追加させます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のより良いカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
