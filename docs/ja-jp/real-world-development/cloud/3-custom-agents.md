---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象タスクに役立つ指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用できるエージェントにまとめることもできます。たとえば、[WCAG][wcag] に沿ったアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を置き、その後にエージェントの動作、専門知識、指示を記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと[agent skill][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整できます。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業でプルリクエスト ワークフローのスキルやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もが使えるクラウドファンディング プラットフォームを目指しています。最近、現在のダーク テーマでは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく作成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認して、Copilot への指示を把握しましょう。

Codespace に戻り、アクセシビリティ エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシビリティ対応 Web サイトのコード生成に関する主要な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

Mission control は、この環境のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を与えられます。まず、ハイコントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェント一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントと Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめることで、Copilot がその指針に沿って作業できます。

このレッスンでは、カスタム エージェントの定義方法と、カスタム エージェントへのタスクの割り当て方を確認しました。

Copilot がハイコントラスト モードを実装している間に、次は mission control から[エージェント セッションを監視して調整します][next-lesson]。

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
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
