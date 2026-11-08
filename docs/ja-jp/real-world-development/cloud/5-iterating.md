---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot に複数の作業を依頼しました。コードへのドキュメント追加、デザイン チームが確認する関連ゲーム機能の作成、ハイコントラスト モードとライト モードを含むアクセシビリティ機能の実装です。コード変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに従います。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード用ブランチ **のみ** に許可されます。
- Cloud agent は GitHub Actions 内に、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がコードにドキュメントを追加した最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開いたとき、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定された動作です。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されています。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は異なります。Exercise 2 の最初に確認したカスタム指示ファイルに基づく、TSDoc コメント、TypeScript 規約、コメント ヘッダーを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに入るはずです。問題がなければ、単一 Astro アプリのプロジェクト チェックは成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、依頼文を固定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、コードベースに必要なドキュメント変更を **@copilot** にメンションしてコメントします。
2. **View Session** を選択して Copilot の作業を確認します。変更用に新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。問題がなければ PR をドラフトからレビュー可能な状態にし、`main` にマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに入り、問題がなければ単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc コメントを追加したりできます (この Issue はカスタム指示を更新する前に割り当てたことを思い出してください)。**Conversation** タブで新しいコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 の mission control で追加したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - ハイコントラストとライト モードの CSS またはスタイル変更。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに入り、問題がなければ単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチを切り替え、アクセシビリティ機能を手動テストできます。ターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストとライト モードを切り替えて、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに沿うよう Copilot を導きました。問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 - さらに作業を委任する

Cloud agent に実際のバックログ項目を渡し、その間に別の作業を進めると便利です。リポジトリに Issue を追加して Copilot に割り当て、この習慣を身につけましょう。たとえば次のような作業があります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで次のことを行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力に及ぼす影響を確認しました。
- Issue を Copilot cloud agent に割り当て、環境の設定、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイコントラストとライト モードを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して指示しました。
- `@copilot` に変更を依頼し、ワークフローを承認して、cloud agent の PR をレビュー、改善しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスでは、今回の学習内容をさらに広げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリで、次のことも試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

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
