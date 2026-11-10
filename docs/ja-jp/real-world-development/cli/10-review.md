---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みの絞り込み Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始め、Plan モードで実装方法を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズしてユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 成果

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作成しました。

1. **星評価**: 既存の `starRating` と未評価状態の表示をゲーム カードに追加しました。
2. **絞り込みと品質ワークフロー**: 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポート、QA プロファイル、関連テストを追加しました。

絞り込みの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、絞り込みと品質ワークフローの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー確認を実施しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい書式で結果を報告しました。QA はこれらの結果を、要件とテスト カバレッジのレビューにまとめました。

追加したテストで実際の不足を補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと証拠を確認し、変更後は関連する証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。アプローチを事前に検討するには **Plan**、小さな変更を自分で確認しながら進めるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選択します。

コンテキストは、インフラストラクチャと同じくらい重要です。何を、なぜ、どのように作るかを明確に伝えることで、生成結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI の次の機能も確認できます。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

どのツールも、使い続けることで習熟できます。本番コード、趣味のコード、いつか作りたいと思っていた小さなアプリなどで活用してください。チームで学んだことを共有し、ほかの開発者の経験からも学びましょう。必要に応じてドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの作業環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
