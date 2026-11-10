---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、指示、ベスト プラクティス、コーディング パターン、専門知識を定義します。チームの知識を、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、テスト パターンを統一するテスト エージェントなどとして再利用できます。

カスタム エージェントはプロジェクトの `.github/agents`、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの動作、専門知識、指示を Markdown で記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] は、どちらも主に Markdown ファイルで定義し、AI にタスクの実行方法を伝えます。分かりやすく区別すると、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、skill やほかのエージェントを組み合わせて作業します。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、PR ワークフローやテスト実行用の skill を使うこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、使う環境やシナリオに合わせて試し、改善してください。

この演習では、カスタム エージェントの定義方法とタスクの割り当て方法を確認します。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。利用者から、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいという意見が寄せられました。デザイン チームは、利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントに機能を作成させて、速やかに実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントが用意されています。Copilot をどのように導くか確認しましょう。

Codespace に戻り、アクセシビリティ エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する責任、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

mission control は、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、指示を追加できます。まず、ハイ コントラスト モードの作成を依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選び、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントとして Accessibility agent を選んだ割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot のカスタム エージェントを確認しました。チームの知識や標準を再利用可能なエージェントにまとめ、特定のタスクや分野で Copilot を導けます。カスタム エージェントの定義方法とタスクの割り当て方法を確認しました。

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して方向を調整][next-lesson]します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
