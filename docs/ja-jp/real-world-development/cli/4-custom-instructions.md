---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示とパスに応じた指示ファイルを確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業方法が決まっている場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も伝えられます。絞り込み機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys では、次のような開発ガイドラインを定めています。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数がある場合は、注入可能であることも記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも更新します。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝えられます。推奨する規約やライブラリ、コードに含めるコメントなどを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルに適用できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべての** リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関するガイドラインを提供します。

> [!NOTE]
> 他の指示形式への対応状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが用意されています。変更の影響を確かめる前に、既存の内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます (ターミナルではありません)。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob によって、指示の適用対象が決まります。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が従う例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ改善できます。新しく生成する TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` を開きます。35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するため、絞り込み機能の会話を終了して再開します。機能のコンテキストは維持されます。

1. 絞り込み機能の会話を開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチにいることを確認し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合は、絞り込み機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のプロンプトを送信し、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング規約を守るため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を書くためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
