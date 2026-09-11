# igei

IGEI Inc.｜医療藝術融合会社 公式サイト → https://igei-med-art.com/ （公開リポジトリ: igei-med-art/igei-med-art.github.io。newnet.jp/igei は旧URL）

- `index.html` — 薬箱ランディング。ロゴ正面（平面に見える）→前傾→水平回転→裏面→プルタブ（白線）から開封→
  画面下部へ移動→錠剤シート(MENU)と展開前の添付文書（支給画像）が射出。
  錠剤の各項目は添付文書の該当セクションへ、添付文書 / MORE ▶ は添付文書へ、◀ PACKAGE で最初の画面へ
  （箱はドラッグで全方向回転）。指示書(9/9)どおり箱は小さく中身は大きく（`--bs` 箱倍率 / `--blw` シート幅 / `--pw` 文書幅）。
  ◀ PACKAGE ／ MORE ▶ の級数は錠剤シートの MENU と同じ（シート幅×0.106）。
  箱の6面は最終データPDFの切り出し画像（`img/box-*.png`、前面もロゴ画像）。比率 340×204×84。
  底面だけ画像を180°回転して保存してある（`.bottom` の transform の都合）。
- ロゴは支給データそのままのアルファPNG `img/logo-mask.png` を CSS mask にして色を付ける（`.logo`）。SVGはfaviconのみ。
- `insert.html` — 添付文書。項目名は「二字熟語・英語」。右上の二字熟語表と上部バーの英語名はどちらも
  添付文書内の各項目へジャンプ。各項目は概要数行だけ載せ、MORE で詳細ページへ。
  右上に「=QUAL M=DICAL C=NT=R」の吊り下げ看板（添付文書モードのみ常時表示）→ https://igei.base.shop
- `detail.html?s=<項目>` — 各項目の詳細ページ。角丸のタイトル（二字熟語 / 英語）＋記事ボックス。
  TOP で添付文書の最上部へ。ここには吊り下げ看板を出さない。
- `content.json` — 編集可能なコンテンツ。`site`（サイト名・description・URL・OGP画像・SNS・ショップ・noindex・GA・
  Search Console）/ `pages`（ページ別 title・description）/ `index`（薬箱画面の文言）/ `paper`（添付文書の固定文）/
  `labels`（項目名 二字熟語・英語）を admin から編集。各ページは読み込み時に `data-t` / `data-label` 等の要素へ反映する。`sections.<項目>.summary`（概要）と `articles`（記事: title / sub /
  date / text / images[] / link / updated）。【近況・RECENT WORKS】は全項目の記事のうち `updated` が新しい3件を
  自動表示し、記事の先頭画像をサムネイルにする。
- `admin.html` — 更新用GUI（公開ページからは非リンク）。保存時に content.json のほか、3ページの `<head>` の
  `<!-- seo:start -->`〜`<!-- seo:end -->`（title / description / canonical / OGP / twitter card / JSON-LD /
  GA / 所有権確認）と `sitemap.xml` / `robots.txt` を生成して直接コミットする（変更があるファイルだけ）。
  head のこのブロックは手で編集しない。
- `img/` — 支給素材と admin からアップロードした画像

## 更新方法（GUI）

1. https://igei-med-art.com/admin.html を開く（HTTPS証明書が発行されるまでは http:// で）
2. 初回のみ GitHub Fine-grained トークンを作成して保存
   （Repository access: igei-med-art/igei-med-art.github.io ／ Permissions: Contents: Read and write。
   admin は開いたドメインからコミット先リポジトリを自動で選ぶ）
3. フォームで編集 →「保存して公開」→ 約1分で反映

admin は GitHub Contents API で `content.json` と `img/` を直接コミットする。
insert.html / detail.html が読み込み時に `content.json` を fetch して反映する。
