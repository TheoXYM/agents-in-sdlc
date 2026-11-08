---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法が決まっている場合や、Copilot が知っておくべき背景情報がある場合は、Copilot がその情報を使えるようにします。[指示ファイル][instruction-files]は、コード生成の指針を伝える方法の 1 つです。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルが、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- *現在の*指示で、絞り込み用の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が更新済みの指針を使えるよう、指示と絞り込み機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが指定した標準に従わない場合もあります。Copilot は確率的に動作します。この演習では、出力の文字単位の一致ではなく、指示の更新後に動作が変化する傾向を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発方法のガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot に伝えるコンテキストです。推奨する規約やライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体、または特定のファイル形式やタスクに指示を適用できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストに送る単一の指示ファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式用に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには代表的な方法を示す例が含まれています。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル形式や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。望ましいコードと避けるべきコードの例も示します。

指示ファイルの作成に唯一の方法があるわけではなく、AI の使い方も同様です。プロジェクトに合わせて試しながら見つけてください。

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

## このプロジェクトのカスタム指示を確認する

まず、このリポジトリに用意された指示ファイルを読みます。中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` があります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの短い説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も含め、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリ ルートからの相対 glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を示せます。ここで使うパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内ドキュメントの要件はまだありません。次に TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順で説明します。

カスタム指示の効果を確かめるため、まず現在の指示でプロンプトを送ります。後で指示を更新し、同じプロンプトを再送して違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストのみを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパー ファイルとテストなどを変更します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に受け取る型付き関数で、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードには TSDoc コメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後の動作が一貫するかを確かめます。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コード提案を改善するため、リポジトリのコーディング標準にルールを追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。生成されたヘルパーにドキュメント コメントがなかった理由として、コード内ドキュメントの規則がまだないことを確認します。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル ヘッダーと TSDoc コメントを付けるよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示は `.github/copilot-instructions.md` に、言語やファイル形式、タスク固有の指示は `*.instructions.md` に追加できます。ここで追加したドキュメント コメント規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で利用する実際のコードに適用できました。

## 指示とブランチをコミットして push する

指示ファイルも他の資産と同様にソース管理で管理します。指示とブランチをコミットしてリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示とヘルパーの変更があることを確認します。
3. 指示の変更と、ヘルパーの基礎作成にあたって Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したら、バックログにある指示ファイル更新の Issue と関連付けてプルリクエストを作成します。GitHub ツールを使える Copilot に、Issue の検索、PR の作成、マージを依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールが設定済みなので、追加の設定は不要です。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習では Model Context Protocol による接続方法を学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** を選びます。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成して、指示の更新と発行元ヘルパーの基礎作成を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して適切に **Allow** を選びます。
5. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作やシェル コマンドの実行許可を求めたら、コマンドを確認して適切に **Allow** を選びます。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、指示とヘルパーの基礎をコミットして `main` にマージしました。

次は[エージェント モード][next-lesson]で、コードベース全体にわたる新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示作成のベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
