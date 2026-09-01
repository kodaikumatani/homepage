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
└── CNAME           独自ドメイン設定（GitHub Pages が参照）
```

## ローカルで確認する

`index.html` をブラウザで直接開くだけでも動きますが、パスの挙動を本番に揃えるならローカルサーバー経由が確実です。

```sh
python3 -m http.server 8000
# http://localhost:8000
```

## 公開

`main` ブランチに push すると GitHub Pages が自動で再デプロイします。

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
