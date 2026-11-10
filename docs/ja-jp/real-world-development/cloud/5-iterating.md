---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot といくつかのタスクに取り組み、ユーザー体験を改善して機能を追加しました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を作り、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コード変更を確認し、必要に応じて Copilot に改善を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の開発プロセスを実行します。チームのレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期で、開発者が監視していない間も作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内に、作業ごとに破棄される独立した環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。[外部アクセスの制限][agent-firewall]を参照してください。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した、コードのドキュメントを追加する最初のプルリクエスト (PR) を GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへのアクセスなど、外部リソースへのアクセスを既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があるため、その間にこれまでの内容を振り返ってください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TypeScript のドキュメント コメント (TSDoc) やドキュメントを確認します。変更内容は状況によって異なります。演習 2 で確認したカスタム指示に基づく TSDoc コメント、TypeScript の規約、ファイル先頭のコメントが含まれているか確認してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されることを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーに依頼するように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを受け取り、修正のための新しいセッションを開始します。結果は確定的ではないため、必ず特定の文面を使う必要はありません。次のような依頼ができます。

- 各コード ファイルの先頭に、役割を説明するコメントを追加する。
- TypeScript と Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. ドキュメント変更を依頼するコメントを追加し、ユーザーと同じように **@copilot** をタグ付けします。上記の例、または追加したいドキュメントについて別の案を使います。
2. **View Session** を選択し、Copilot が更新する様子を確認します。変更用に新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しい commit が追加されていることを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで Copilot に変更を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` にマージします。

![ドラフト PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルの PR を選択します。
4. **Files changed** タブを選んで生成されたコードをレビューします。
5. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されることを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能が作成されました。ほかの PR と同様に、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示するゲーム数の調整、ファイル先頭のコメントや TSDoc コメントの追加を依頼できます。この Issue はカスタム指示を更新する前に割り当てたものであることに注意してください。変更を依頼するには **Conversation** タブにコメントを追加します。Copilot がコメントを受け取り、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、カスタム アクセシビリティ エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの利用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを制御する JavaScript/TypeScript コード。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されることを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。ターミナルで Copilot が作成したブランチ名に置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ハイ コントラストとライト モードの切り替えをブラウザーで試します。ページを再読み込みしても設定が保持されることを確認します。

カスタム アクセシビリティ エージェントは、アクセシビリティのベスト プラクティスに沿うよう Copilot を導きました。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 - さらにタスクを委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進める練習をしましょう。リポジトリに Issue を追加し、Copilot に割り当てます。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援への関心を伝えるフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで次のことを行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の結果への影響を確認しました。**
- **Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して途中で方向付けしました。**
- **Copilot のプルリクエストをレビューして反復し、`@copilot` に変更を依頼し、ワークフローを承認しました。**

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、ここで取り組んだ内容を別の環境で発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の方法も試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けにカスタム エージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
