---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、次のレッスンで Copilot CLI をインストールして実行するための統合ターミナルを備えた [Codespace][codespaces] を起動します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレート リポジトリ][tailspin-template]からコピーを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、次に **Create a new repository** を選択して、自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 後で使用するため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこの Issue に取り組むため、自分で登録する必要はありません。

ワークショップ テンプレートの新しいコピーを使用してください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、バックログが含まれています。古いコピーを使用する場合は、必要なファイルがあるか進行役に確認してください。

## Codespace を作成する

次に、ワークショップの演習に使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーからコードを記述、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した高機能なエディターを利用できます。

1. 作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備が整いました。このレッスンでは、テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成し、Codespace を用意してプロジェクトを確認しました。

次は、Codespace に[GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
