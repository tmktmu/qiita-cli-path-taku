---
title: README（Qiita投稿対象外）
tags:
  - README
private: true
ignorePublish: true
---

# Qiita 記事管理リポジトリ

[Qiita CLI](https://github.com/increments/qiita-cli) を使って、Qiita の記事を Markdown ファイルで管理・プレビュー・投稿するためのリポジトリです。

> [!NOTE]
> Qiita CLI は `public/` 配下の `.md` をすべて記事として扱います。
> この README も記事として読み込まれるため、先頭の Front Matter で `ignorePublish: true` を指定して投稿対象から除外しています。削除しないでください。

## ディレクトリ構成

```
C:\Qiita\
├── .github/workflows/publish.yml  # GitHub Actions による自動投稿
├── node_modules/
├── package.json                   # @qiita/qiita-cli を devDependencies に登録
├── qiita.config.json              # Qiita CLI の設定
└── public/                        # 記事フォルダ（このリポジトリ）
    ├── README.md
    ├── *.md                       # 記事ファイル
    └── .remote/                   # Qiita 上の記事のコピー（CLI が管理）
```

`qiita` コマンドは **`C:\Qiita`（public の親フォルダ）で実行**してください。

## セットアップ

```powershell
cd C:\Qiita

# Qiita CLI のインストール（初回 / 別PCで環境を作るとき）
npm install --save-dev @qiita/qiita-cli

# バージョン確認
npx qiita version

# 初期化（設定ファイルや workflow を生成。済んでいれば不要）
npx qiita init

# ログイン（Qiita のアクセストークンを入力）
npx qiita login
```

アクセストークンは Qiita の [設定 > アプリケーション](https://qiita.com/settings/applications) から発行します（スコープ: `read_qiita`, `write_qiita`）。

## コマンド一覧

| やりたいこと | コマンド |
| --- | --- |
| プレビューを起動（http://localhost:8888） | `npx qiita preview` |
| 新しい記事ファイルを作成 | `npx qiita new 記事ファイル名` |
| 記事を投稿・更新 | `npx qiita publish 記事ファイル名` |
| すべての記事を投稿・更新 | `npx qiita publish --all` |
| Qiita 上の記事を手元に取得 | `npx qiita pull` |
| Qiita 上の記事で手元を強制上書き | `npx qiita pull --force` |
| ログイン | `npx qiita login` |
| バージョン確認 | `npx qiita version` |
| ヘルプ | `npx qiita help` |

※ 記事ファイル名は拡張子 `.md` を付けずに指定します。

## 記事投稿の流れ

```powershell
cd C:\Qiita

# 1. 記事ファイルを作成 → public/my-article.md ができる
npx qiita new my-article

# 2. プレビューを起動してブラウザで確認しながら執筆
npx qiita preview

# 3. Qiita に投稿
npx qiita publish my-article
```

初回投稿後、Front Matter に `id` と `updated_at` が自動で書き込まれます。以降は同じコマンドで記事が更新されます。

## Front Matter（記事ヘッダー）

```yaml
---
title: 記事タイトル
tags:
  - Qiita
  - QiitaCLI
private: false          # true で限定共有記事
updated_at: ''          # 投稿時に自動設定（編集しない）
id: null                # 投稿時に自動設定（編集しない）
organization_url_name: null
slide: false            # true でスライドモード
ignorePublish: false    # true で publish の対象外にする（下書き用）
---
```

- **下書き**: `ignorePublish: true` にしておくと `publish --all` でも投稿されません。
- **限定共有**: `private: true` にすると限定共有記事として投稿されます。
- **タグ**: 1〜5個まで指定できます。

## GitHub Actions による自動投稿

`.github/workflows/publish.yml` により、`main`（または `master`）ブランチへ push すると記事が自動で Qiita に投稿・更新されます。

1. GitHub リポジトリの **Settings > Secrets and variables > Actions** に `QIITA_TOKEN` を登録
2. 記事を編集してコミット & push

```powershell
git add .
git commit -m "記事を追加"
git push origin main
```

> [!WARNING]
> workflow ファイルは `.github/workflows/` がリポジトリのルートにないと動作しません。
> また `root` には `public/` フォルダを含むディレクトリを指定する必要があります。

## 参考リンク

- [Qiita CLI（GitHub）](https://github.com/increments/qiita-cli)
- [Qiita CLI の README（コマンド詳細）](https://github.com/increments/qiita-cli/blob/main/README.md)
- [Markdown 記法 チートシート（Qiita）](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)
