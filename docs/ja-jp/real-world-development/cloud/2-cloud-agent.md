---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、セキュリティの問題、更新が必要なレガシー コード、時間がなくバックログに残った機能要望など、技術的負債が課題になっています。GitHub Copilot cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- Issue の範囲を明確にする重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。Web サイトの初期版を作成した委託先は、ドキュメントをほとんど残していません。まず、アプリケーションのエクスポートされた関数に TypeScript のドキュメント コメント (TSDoc) を追加します。

デザイン チームは、ゲームの詳細ページに同じカテゴリのほかのゲームを表示し、利用者がゲームを見つけやすくしたいと考えています。完成度の高い実装ではなく、ユーザー体験 (UX) の受け入れテストに使える実装を求めています。この作業はブロッカーですが、さらに優先度の高い Issue もあります。

どちらも優先順位が下がりやすいタスクで、Copilot cloud agent に適しています。非同期で作業を任せている間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当て後、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備して作業を始めます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、PR で開発者に通知します。

## 範囲を明確にすることの重要性

GitHub Copilot に任せれば考える必要がなくなるわけではありません。一見簡単な作業でも、詳細を検討すると複雑な場合があります。[cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学びながら試して調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot を導く仕組みを確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで読み取り専用で確認しておきます。後から cloud agent の作業に指示が反映されているかを確認できます。

GitHub の Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する内容を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルを対象にしていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、プルリクエストに TSDoc コメント、TypeScript の規約、ファイル先頭のコメントが反映されているかを確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するタスクでは、通常、適切な状態で作業するための環境やセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。

cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できるよう、あらかじめ設定しています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリを複製する場合は `contents: read` 権限が必要です。複製しない場合は、手順の完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と同様ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でワークフローを実行し、正常に動くか確認できます。

## ドキュメントを追加する

多くのプロジェクトには、古くなった情報や不足しているドキュメントがあります。これは生産性を下げ、コードベースの保守や新しい開発者の参加を難しくする技術的負債です。Copilot はドキュメントの作成に役立つため、このタスクを cloud agent に割り当てます。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com の自分のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。PR が一覧にない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> todo リストが PR に表示されるまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を準備し、プロジェクトを分析して、作業の進め方を決めています。

13. todo リストを確認し、Copilot が実行するタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブ セッションをスクロールし、Copilot がコードを調査して状態を把握し、計画を考えてコードを作成する様子を確認します。

この作業には数分かかる場合があります。cloud agent の主な利点は、非同期にタスクを実行している間に別の作業を進められることです。もう 1 つのタスクを割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent を使うと、作業を分け、エージェントに別のタスクを任せている間に開発者がほかの作業を進められます。関連ゲームの表示は短時間でできるとしても、その時間を別の作業に使えます。次の Issue も cloud agent に割り当てます。

1. github.com の自分のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成ダイアログを開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選び、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Copilot が作業を始めると、Issue の最初のコメントに目の絵文字が表示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のタスクにも取り組み始めました。cloud agent はソフトウェア エンジニアのように作業するため、常に監視する必要はありません。完了後に結果をレビューします。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てて、非同期に作業させることができます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- Issue の範囲を明確にする重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前の演習で扱ったカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
