# hibi-zakka Design Learnings

## 目的

zakkaペルソナ（手しごと好きの20代女性アパレル店員）は、ナチュラル系雑貨ECの「テンプレ化したアースカラー＋丸ゴシック」に飽きている。SNS で出会う個人店の手触りを Web で再現するため、手書き ・ 紙コラージュ ・ 蛍光ペンの3つを軸にした。

このブランドは架空店舗であり、実在の店舗・商品ではない。

## 設計したこと

- ヒーローを「手紙風挨拶 + ポラロイド3枚 + マスキングテープ」のスクラップブック構成にし、SNS 投稿を Web にしたような第一印象を作った。
- 商品グリッドの各カードに、3色のマスキングテープを `:nth-child(3n+1/2/n)` で順に貼り付けた。
- 「催し」セクションは月・日・曜日のカードを左に置き、雑誌「ku:nel」の連載風UIに。
- 店主の手帖はノート罫線 + 左の朱の縦線 + 蛍光ペンの強調で、ノートに書き付けたエッセイを再現。

## CSS

主要なCSS判断:

```css
:root {
  --kinari:#f7f1e8;
  --matcha:#697764;
  --shu:#c4513a;
  --benibara:#d9a89d;
  --font-tegaki:'Klee One','Sawarabi Mincho',serif;
}

/* マスキングテープ（3色を順に） */
.card:nth-child(3n+1) .tape {
  background: repeating-linear-gradient(45deg,
    var(--shu) 0, var(--shu) 4px,
    var(--shu-asa) 4px, var(--shu-asa) 8px);
}

/* ノート罫線（背景） */
.techo {
  background:
    repeating-linear-gradient(0deg,
      transparent 0, transparent 38px,
      rgba(122,109,82,.22) 38px, rgba(122,109,82,.22) 39px),
    var(--kinari);
}

/* 蛍光ペン強調 */
.fluo {
  background: linear-gradient(transparent 60%,
    var(--benibara) 60%, var(--benibara) 92%, transparent 92%);
}
```

## アニメーション

- `float-in` リビールを 15px / 1s で軽快に。
- 商品カードホバーで `translate(-2px,-2px)` + 影強化、「持ち上がった」気配を残す。
- ナビホバーで蛍光ペンの背景が一瞬 skew して現れる遊び。

## フォント

- 主書体: `Klee One`（教科書体、親しみの源泉）
- サブ見出し: `BIZ UDPMincho`（UD系明朝、可読性）
- アクセント: `Yuji Syuku`（落款的味）
- かな: `Sawarabi Mincho`

英字フォントは未採用。「友人の手紙」に英字書体は不自然だから。

## ボタン

ボタンはステッカー的に作る。

- 「お便りを読む」「催しに申し込む」は線と文字のみ。塗りボタンは「店の案内」CTA だけに限定。
- リンク下線は普通の `text-decoration` を避け、`::after` で蛍光ペン色の帯を入れる。

## UI/UX

- ナビ4項目（お店の品 / お便り / 催し / 手帖）。「手帖」がエッセイ、店主の人格に触れるルート。
- 商品カードに「マスキングテープ」「ハンコ風カテゴリ」「価格バッジ」の3つを必ず配し、紙のメモを集めたような体裁を維持。
- 「催し」では月日カードを左に大きく、参加費を末尾に書く——雑誌の連載告知UIを参照した。

## レイアウト

ポラロイドは絶対配置で重ね、回転とz-indexを微妙にずらして「貼った位置のずれ」を表現。3枚のうち2枚はモバイルでは縦並びに（`min-height: 340px` のコンテナで自動再構成）。

セクションごとに「ハンコ」「リボン」「蛍光ペン」「ノート罫線」のうち1つを必ず使い、紙の手触りを通底させた。
