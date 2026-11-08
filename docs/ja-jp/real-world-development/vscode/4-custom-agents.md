---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象タスクに役立つ指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用できるエージェントにまとめることもできます。たとえば、[WCAG][wcag] に沿ったアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を置き、その後にエージェントの動作、専門知識、指示を記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと[agent skill][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整できます。この演習のアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビュー、更新します。その作業でプルリクエスト ワークフローのスキルやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もが使えるクラウドファンディング プラットフォームを目指しています。最近、現在のダーク テーマでは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく作成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認して、Copilot への指示を把握しましょう。

Codespace に戻り、次のコマンドで `main` からアクセシビリティ作業用のブランチを作成します。Exercise 3 の絞り込み PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシビリティ対応 Web サイトのコード生成に関する主要な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義されたカスタム エージェントが表示されます。カスタム エージェントを選択すると、その指示とツールに基づくチャット セッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 手順を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要なときに説明します。

1. Chat ビューのエージェント ドロップダウンから、まだ選ばれていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェント名が表示されます (既定では **default**)。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクト ファイルを編集します。編集はファイル パスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業ファイルの一覧は、**Files changed** の表示で確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリを変更しながら、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業を確認して指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめることで、Copilot がその指針に沿って作業できます。

この演習では、カスタム エージェントの定義方法と、Copilot Chat のエージェント モードでの使い方を学びました。

次は[エージェントの作業を監視して調整し][next-lesson]、変更を確認しながら同じセッションにライト モードを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
