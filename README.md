# homepage

静的な個人ホームページ。HTML / CSS / JavaScript のみで構成され、ビルド不要で GitHub Pages から配信されます。

公開先: **https://kodaikumatani.github.io/homepage/**

## 構成

```
.
├── index.html      1ページ完結のトップページ
├── css/style.css   スタイル（CSS 変数でテーマを一括変更）
├── js/main.js      ヘッダー追従・メニュー開閉・フェードイン
├── images/         画像置き場
├── CNAME           独自ドメイン設定（GitHub Pages が参照）
└── .github/workflows/
    ├── deploy.yml      main → 本番へ配信
    └── pr-preview.yml  PR → プレビューへ配信
```

ブランチは2本です。`main` がソース、`gh-pages` が配信物（Actions が自動更新するので手で触りません）。

```
gh-pages/
├── index.html          ← 本番（main の内容）
└── pr-preview/
    └── pr-3/           ← PR #3 のプレビュー
        └── index.html
```

## ローカルで確認する

`index.html` をブラウザで直接開くだけでも動きますが、パスの挙動を本番に揃えるならローカルサーバー経由が確実です。

```sh
python3 -m http.server 8000
# http://localhost:8000
```

## 変更の流れ

`main` は保護されており直接 push できません。変更は必ず PR 経由です。

```sh
git switch -c update-works
# 編集
git commit -am "Update works section"
git push -u origin update-works
gh pr create --fill
```

PR を作ると数十秒でプレビュー URL が PR にコメントされます。実機・スマホでも確認できます。

```
https://kodaikumatani.github.io/homepage/pr-preview/pr-<番号>/
```

以降 PR に push するたびにプレビューが更新され、PR を閉じるかマージすると自動削除されます。

```sh
gh pr merge --squash --delete-branch
```

マージすると `main` への push をトリガーに本番が再デプロイされます。

### プレビューが動く条件

- レビュー承認は不要（承認数 0 設定）ですが、PR は必須です
- fork からの PR ではトークン権限の都合でプレビューは動きません（個人リポジトリなので通常は影響なし）

## テーマの変更

配色は `css/style.css` 冒頭の `:root` にまとまっています。アクセントカラーを変えるだけで全体の印象が変わります。

```css
:root {
  --accent:        #6ea8fe;
  --accent-strong: #3d7dfc;
}
```

## 独自ドメインの設定

1. `CNAME` にドメイン名を1行だけ書く（例: `example.com`）
2. DNS 側にレコードを追加する

**サブドメイン（`www.example.com` など）の場合** — CNAME レコード1件

| Type | Name | Value |
|---|---|---|
| CNAME | www | `kodaikumatani.github.io` |

**Apex ドメイン（`example.com`）の場合** — A レコード4件

| Type | Name | Value |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

3. DNS が伝播したらリポジトリの Settings → Pages で **Enforce HTTPS** を有効にする（証明書の発行に最大24時間ほどかかります）

## 書き換えが必要な箇所

公開前に以下を実際の内容に差し替えてください。

- `index.html` の `mailto:example@example.com` — 連絡先メールアドレス
- Works セクションの `href="#"` — 各リポジトリや記事の URL
- About / Skills の文面

