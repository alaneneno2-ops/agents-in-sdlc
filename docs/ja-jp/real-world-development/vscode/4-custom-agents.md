---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、Copilot を導く具体的な指示、ベスト プラクティス、コーディング パターン、専門知識を定義します。チームの知識を再利用可能なエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、テスト パターンを統一するテスト エージェントを作成できます。

カスタム エージェントはプロジェクトの `.github/agents`、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの動作、専門知識、指示を Markdown で記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] は、どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。分かりやすく区別すると、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、skill やほかのエージェントを組み合わせて作業します。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローやテスト実行用の skill を使うこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、使う環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。利用者から、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいという意見が寄せられました。デザイン チームは、利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って機能をすばやく作成します。

この演習では、アクセシビリティ用のカスタム エージェントを確認し、Copilot Chat で使ってハイ コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントが用意されています。Copilot をどのように導くか確認しましょう。

Codespace に戻り、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 の絞り込み PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する責任、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたカスタム エージェントが表示されます。エージェントを選ぶと、そのエージェントの指示とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な手順は各所で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを開きます。既定のエージェントは **default** と表示されています。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集する様子を確認します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が進むと、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更しています。

次の演習では、進行中の作業をレビューし、エージェントに追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。チームの知識や標準を再利用可能なエージェントにまとめ、特定のタスクや分野で Copilot を導けます。この演習では、カスタム エージェントの定義方法と、Copilot Chat の agent mode での使用方法を確認しました。

次は[エージェントの作業を監視して方向を調整し][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

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
