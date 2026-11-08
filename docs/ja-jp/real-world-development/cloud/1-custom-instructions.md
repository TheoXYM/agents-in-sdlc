---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法が決まっている場合や、Copilot が知っておくべき背景情報がある場合は、その情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示の変更前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue で非同期に動作するため、影響をリアルタイムに並べて示すのが難しいためです。後の演習で cloud agent が作成した PR を確認する際に、指示ファイルの効果を見ます。

## 指示ファイル

### シナリオ

Tailspin Toys には開発方法に関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot に伝えるコンテキストです。推奨する規約やライブラリ、コードに含めるコメントなどを指定でき、リポジトリ全体または特定の種類のファイルに適用できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストに送信する単一の指示ファイルです。技術スタック、プロジェクト概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの特定の言語、UI コンポーネントの作成、ユニットテストの追加といったファイル形式やタスクに関する指針を記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは、指示ファイルの作成について詳しく扱いません。サンプル プロジェクトには代表的な方法を示す例があります。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル形式や特定タスクに応じた指示を記載します。
- 自然な言葉で明確に書き、望ましいコードと避けるべきコードの例を示します。

指示ファイルの作成にも AI の使い方にも唯一の方法はありません。プロジェクトに合わせて試してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で方法は異なります。必要な場合は、利用中の環境に対応するレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタム エージェントなどのリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを読みます。中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` があります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの短い説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も含め、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドがリポジトリ ルートからの相対 glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、Copilot に参考例を示せます。ここで使うパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内ドキュメントの要件はまだありません。次に TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体の標準を追加する

この演習で行う編集は、TSDoc コメントとファイル レベルのコメント ヘッダーに関するプロジェクト全体の規則を追加することです。後の演習で Issue を割り当てたとき、cloud agent がこの指示を使います。

編集の前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読みますが、変更をきちんとコミットしておきます。

1. Codespace のターミナルで、ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約は記載されていますが、コード内ドキュメントのルールはまだありません。
4. 既存の標準のすぐ下に次の行を追加し、ファイル ヘッダーと TSDoc コメントを付けるよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示は `.github/copilot-instructions.md` に、言語やファイル形式、タスク固有の指示は `*.instructions.md` に追加できます。ここで追加したドキュメント コメント規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、push、マージする

Cloud agent は Issue の対象ブランチにある指示ファイルを読みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の更新を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に取り組むとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップでは、`main` に直接作業してブランチを省略してもかまいません。実際のプロジェクトで行う方法に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。以降の演習で cloud agent が作成するプルリクエストに、この標準が反映されます。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示作成のベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
