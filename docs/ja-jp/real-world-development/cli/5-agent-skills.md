---
title: "レッスン 5 - エージェント スキルを使う"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリ開発では、ビルドの生成、テストの実行、プルリクエストの作成など、繰り返し実行するタスクがよく発生します。**Agent skill** を使うと、Copilot やほかの AI エージェントに、こうしたタスクの進め方を伝えられます。skill は、エージェントが必要に応じて読み込む指示、スクリプト、リソースをまとめたフォルダーです。[Agent Skills はオープン標準][agent-skills-repo]で、さまざまなエージェントが利用しています。同じ skill を Copilot Chat の agent mode、Copilot cloud agent、Copilot CLI、GitHub Copilot app で共有できます。

skill はプロジェクトの `.github/skills` フォルダー、またはユーザー共通の `~/.copilot/skills` に配置します。各 skill はフォルダーで、`name` と `description` を含む YAML frontmatter を持つ `SKILL.md` と、その後に続く Markdown の指示で構成されます。

```yaml
---
name: make-contribution
description: All changes to code must follow the guidance documented in the repository. Before any issue is filed, branch is made, commits generated, or pull request (or PR) created, a search must be done to ensure the right steps are followed. Whenever asked to create an issue, commit messages, to push code, or create a PR, use this skill so everything is done correctly.
---
```

skill には、スクリプト、アセット、参考資料を含むサブフォルダーも追加できます。全体の構成は [agent skills specification][agent-skills-spec] で説明されています。

> [!TIP]
> skill は動的に読み込まれます。どの skill を適用するかは、エージェントが `description` フィールドを基に判断します。明確で具体的な説明を書くことが、使われる skill と無視される skill の違いにつながります。

これから、チームのプルリクエストが定められた要件に従うよう、skill を使う方法を確認します。

## シナリオ

チームではプルリクエスト (PR) に次の要件を定めています。

- 明確な commit message を使い、ファイルを論理的な単位にまとめます。
- PR を作成する前に、すべてのテストが成功している必要があります。
- 各 PR に次のセクションを含めます。
  - 変更の理由。
  - 変更したファイルの概要。
  - 重要なコード ブロックの抜粋。
  - 変更内容の詳しい説明。

チームでは Copilot を使ってコードや PR を作成しているため、AI ツールにもこれらの要件に従わせたいと考えています。

このレッスンでは、次のことを行います。

- PR 作成用に既存の skill を確認します。
- AI エージェントが skill を利用する仕組みを学びます。
- skill を使ってガイドラインに沿った PR を作成します。

## skill を実行する

skill は、エージェントが必要と判断したときに動的に読み込まれます。どの skill を使うかは `SKILL.md` の description で判断するため、用途を明確に記述することが重要です。

## PR 用の skill を確認する

Tailspin Toys には PR 作成の要件があるため、AI ツールがガイドラインに沿って PR を作成できるよう skill が用意されています。動作を理解するために、内容を確認しましょう。

1. `.github/skills/make-contribution/SKILL.md` を開きます。
2. `name` と `description` を確認します。description には、PR や commit の作成を求められたときに skill を使う状況が書かれていることに注目してください。
3. skill 全体を読みます。ブランチの作成、commit の方法、PR に含める内容のルールを確認します。

## skill を使う

前述のとおり、Copilot CLI は skill を自動的に呼び出します。Copilot に PR の作成を依頼して、動作を確認します。

> [!TIP]
> **Copilot CLI セッションを開始する**
>
> 以下の手順を始める前に Codespace に戻り、ターミナルを開きます (まだ開いていない場合は <kbd>Ctrl</kbd>+<kbd>\`</kbd>)。次に、`--yolo` と `--enable-all-github-mcp-tools` を指定して Copilot CLI を起動します。
>
> ```bash
> copilot --yolo --enable-all-github-mcp-tools
> ```
>
> 新しく開始する代わりに、このプロジェクトの直近のセッションを再開するには `copilot --yolo --enable-all-github-mcp-tools --continue` を実行します。前のレッスンから Copilot CLI が起動したままの場合は、`/clear` を実行して新しい会話を始めてください。
>
> `--enable-all-github-mcp-tools` を指定すると、現在のセッションで GitHub MCP の読み取り / 書き込みツールが有効になります。これにより、ワークショップ中に Copilot がバックログを読み取り、プルリクエストを開けるようになります。

> [!CAUTION]
> `--yolo` を指定すると、すべての自動承認オプション (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) が有効になります。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

1. 次のプロンプトを使って、Copilot に PR の作成を依頼します。

   ```
   Can you please create a pull request for me!
   ```

2. Copilot はリクエストを受け付け、しばらくすると **make-contribution** skill を使っていることを表示します。
3. Copilot は skill の指示に従い、テストの実行、ブランチと commit の作成、最後に PR の作成を行います。
4. PR が作成されたら、リポジトリに戻って開きます。各セクションが skill のガイドラインに従い、チームの要件を満たしていることを確認します。
5. 次のレッスンに進む前に、ローカル ワークスペースを `main` から新しいブランチに切り替え、絞り込み PR とアクセシビリティの作業を分けます。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-cli
   ```

## まとめと次のステップ

agent skill を使って、文書化された要件に沿った PR を作成できました。次のことを行いました。

- PR 作成用の既存 skill を確認しました。
- AI エージェントが skill を利用する仕組みを学びました。
- skill を使ってガイドラインに沿った PR を作成しました。

この変更は絞り込み機能の PR に含まれます。次は[Playwright MCP サーバーを使って Copilot がサイトを直接操作する方法][next-lesson]を確認します。

## リソース

- [Agent Skills について][about-agent-skills]
- [Agent Skills の仕様][agent-skills-spec]
- [Agent Skills リポジトリ][agent-skills-repo]
- [awesome-copilot の Agent Skills][awesome-copilot-skills]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[about-agent-skills]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[agent-skills-repo]: https://github.com/agentskills/agentskills
[agent-skills-spec]: https://agentskills.io/specification
[awesome-copilot-skills]: https://github.com/github/awesome-copilot/tree/main/skills
[allow-all-warning]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/allowing-tools
