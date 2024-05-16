---
title: "ZennとGitHubを連携する方法~本・スクラップの作成方法まで~"
emoji: "🔗"
type: "tech" # tech or idea
topics: ["Zenn", "GitHub", "連携"]
published: true
---

# ZennとGitHubを連携する方法

ZennとGitHubを連携することで、GitHubリポジトリから直接記事を投稿・更新できるようになります。この記事では、その手順を詳しく解説します。

## 1. GitHubリポジトリの作成

まず、Zennのコンテンツを管理するためのGitHubリポジトリを作成します。

1. GitHubにログインし、リポジトリ作成ページに移動します。
2. リポジトリ名を入力し、「Create repository」をクリックします。

## 2. Zenn CLIのインストール

次に、Zenn CLIを使って記事を作成・管理します。以下のコマンドを実行して、Zenn CLIをインストールします。

```bash
npm install zenn-cli
```

インストールが完了したら、Zennのセットアップを行います。

```bash
npx zenn init
```

これにより、必要なディレクトリとファイルが作成されます。

## 3. GitHubリポジトリとの連携

Zenn CLIで作成したディレクトリを、先ほど作成したGitHubリポジトリにプッシュします。

```bash
git init
git remote add origin https://github.com/yourusername/your-repo.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

## 4. Zennの連携設定

Zennのダッシュボードに移動し、「GitHub連携」の設定を行います。

1. Zennにログインし、ダッシュボードの「GitHub連携」ページに移動します。
2. 「リポジトリを連携する」をクリックし、連携するリポジトリを選択します。

## 5. 記事の作成と公開

GitHubリポジトリとZennの連携が完了したら、記事を作成して公開します。

### 記事の作成

以下のコマンドを実行して、新しい記事を作成します。

```bash
npx zenn new:article
```

作成されたMarkdownファイルに記事の内容を記述します。

### 記事のプレビュー

ローカルで記事をプレビューするには、以下のコマンドを実行します。

```bash
npx zenn preview
```

プレビューを確認し、問題がなければ記事をコミットしてGitHubにプッシュします。

```bash
git add .
git commit -m "Add new article"
git push
```

### 記事の公開

GitHubにプッシュされた記事は、自動的にZennに反映されます。Zennのダッシュボードで記事の内容を確認し、公開設定を行います。

## 6. スクラップの作成

スクラップは短いメモやリンクを共有するのに便利です。Zenn CLIを使ってスクラップを作成する手順は以下の通りです。

### スクラップの作成

以下のコマンドを実行して、新しいスクラップを作成します。

```bash
npx zenn new:scrap
```

作成されたMarkdownファイルにスクラップの内容を記述します。

### スクラップのプレビューと公開

スクラップも記事と同様にプレビューして、GitHubにプッシュすることで公開されます。

```bash
npx zenn preview
```

プレビューを確認し、問題がなければスクラップをコミットしてGitHubにプッシュします。

もし自動で表示されない場合は以下のリンクから直接飛べます。

http://localhost:8000/

戻りたいときには`Ctrl(command) + C` で帰ってこれます。

```bash
git add .
git commit -m "Add new scrap"
git push
```

## 7. 本の作成

Zennでは、複数の記事をまとめて一つの本として公開することもできます。以下に、本を作成する手順を示します。

### 本の作成

以下のコマンドを実行して、新しい本を作成します。

```bash
npx zenn new:book
```

### チャプターの追加

本に含めるチャプターを追加するには、以下のコマンドを実行します。

```bash
npx zenn new:chapter
```

作成されたMarkdownファイルにチャプターの内容を記述します。

### 本のプレビューと公開

本全体をプレビューし、GitHubにプッシュして公開する手順は以下の通りです。

```bash
npx zenn preview
```

プレビューを確認し、問題がなければ本をコミットしてGitHubにプッシュします。

```bash
git add .
git commit -m "Add new book and chapters"
git push
```

## まとめ

ZennとGitHubを連携することで、効率的に記事、スクラップ、本を管理・公開することができます。この記事の手順を参考に、ぜひ連携設定を試してみてください。
