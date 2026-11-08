---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作と用意された絞り込み Issue を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンド ラインから行えるため、作業を任せたり、変更を依頼したりしながら集中して作業できます。最初のステップとして、使い慣れたツールで Copilot CLI をインストールします。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーで絞り込み Issue を見つけます。

## シナリオ

チームは、増え続けるバックログに取り組むため AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められたら、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェル プロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送信され、slash command は会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を 2 回押して確定します。
2. `/help` を入力して、インストール済みのバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot がリポジトリを調べてから回答する様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトで、絞り込み Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を完了した後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルでチェックアウトされている現在のブランチで作業し、自動的には worktree を作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既定の許可範囲外のツールを使う前に確認します。このワークショップでは、`--yolo` を付けて再起動します。Codespace 内で承認プロンプトを省略できる、ユーザーが許可したショートカットです。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM のような分離された環境でのみ使用し、日常的な開発の既定値として alias に設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` によって、後のレッスンで Issue やプルリクエストに使う GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動くことを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` で同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、npm を使って GitHub Copilot CLI をインストールし、GitHub アカウントで認証して、ワークショップのリポジトリを信頼し、簡単な会話を試しました。また、GitHub MCP サーバーで絞り込み Issue を見つけました。

次は[最初の対象を絞った変更][next-lesson]を開始し、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
