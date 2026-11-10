---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

この演習では、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが適切です。ハイ コントラストに加えてライト モードも追加するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域にあるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。たとえば、リポジトリのチェックアウト、作業環境の作成、MCP サーバーのセットアップ、割り当てられたタスクを完了するための各手順などがあります。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選択した進め方によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## cloud agent の作業を調整する

進行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。タイトルは Copilot が付けた名前や作業の状態によって異なります。

   ![mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot のタスクを調整するダイアログが表示された agents ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映したことを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順が終わると、新しい要望を作業に取り込みます。

この処理には数分かかります。待っている間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot agents ページを使い、GitHub Copilot cloud agent のタスクを監視して方向を調整しました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業を確認し、範囲の変更や追加の指示をセッションに送れます。

この演習では、mission control のエージェント ページで cloud agent のタスクを監視し、進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、[Copilot が作成したプルリクエストを反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
