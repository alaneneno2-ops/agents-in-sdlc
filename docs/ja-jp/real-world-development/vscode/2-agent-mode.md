---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリの変更でも、複数のファイルを更新し、テストを実行する必要があることがよくあります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、漏れたファイルを見つけて再度変更します。この作業を繰り返すことになります。

Copilot agent mode を使うと、IDE 内でより自律的に作業できます。エージェントはプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。agent mode で Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> agent mode と cloud agent は名前が似ていますが、異なる用途向けです。agent mode は IDE 内で動作し、すぐにフィードバックを得ながら対話できます。cloud agent はチーム メンバーのように非同期で作業し、Issue とプルリクエストを通じてやりとりします。

この演習では、次のことを学びます。

- Copilot agent mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法。
- Copilot agent mode で UI とデータ レイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習で発行元のヘルパーを追加しました。残りのデータ レイヤー、UI、テストを Copilot agent mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の Web サイトの機能を確認します。

Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。アプリは単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースに直接問い合わせます。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクで単一の Astro 開発サーバーを起動できます。Codespace で次の手順を実行します。

1. 前の演習で使った Codespace に戻ります。ブランチは `main` になっているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. ダイアログが表示されて `http://localhost:4321` を開くよう求められた場合は、**x** を選択して閉じます。
6. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Codespace 上の開発サーバーに接続するプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、主な機能を確認します。

- **Home Page**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な手順は各所で説明します。

Web サイトはすでに動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成したときに用意されたバックログを Copilot に表示させます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。Agent は複数ファイルにまたがる変更を計画・実装し、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップで特定のモデルを推奨する意図はありません。作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案をできるだけ一定にして、演習を進めやすくするためです。ただし、大規模言語モデル (LLM) は確率的に動作するため、手順と異なる提案が返る場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使う場合もありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって有用だと考える順に、Issue を並べ替えていることを確認します。

## 指示ファイルを確認する

コード生成を依頼する前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [user interface (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。内容には次の項目があります。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に応じた指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、複雑な指示を小さな再利用可能なファイルに分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つを更新します。

- データ レイヤー (`src/lib/`) の絞り込みロジックを追加または改善します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot agent mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なデータ レイヤー、UI、テストを見つけて変更を始める様子を確認します。

   ![Copilot がプロジェクト ファイルを調べる様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行います。タスクやコードベースによって数分かかることがあります。作業中にエディターで **Keep** と **Undo** が表示されることがあります。作業が完了すると各変更に **Keep** または **Undo** が表示されるため、進行中に選択する必要はありません。

7. Copilot が求めたら **Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot が作業している間に待ち時間があったり、テストに失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで作業を続けます。

   ![Copilot agent mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot agent mode で Tailspin Toys に機能を追加する方法を確認しました。次のことを学びました。

- Copilot agent mode で UI とデータ レイヤーにまたがる機能を実装する方法。
- Copilot agent mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 発展的な演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装させます。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、固定にするかを決めます。
- 希望する実装を作成できるよう、プロンプトに必要なガイダンスを含めます。
- 必要に応じて Copilot に変更を依頼し、コンテキストを追加します。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
