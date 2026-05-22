# 日々雑貨 — vintage-sundries/zakka Spec

**Status:** Approved
**Author:** torifo
**Created:** 2026-05-23
**Updated:** 2026-05-23

---

## 1. Overview

### Problem Statement
手しごとや古道具に関心を持つ20代女性は、ナチュラル系雑貨ECが「定番デザインの繰り返し」になりがちで、新規参入店の個性が伝わらないと感じている。雑誌「天然生活」のような誌面体験を、Web上で再現する事例が不足している。

### Goal
中目黒の小さな雑貨店という設定で、こけし・古布・ガラス瓶・紙ものを扱う日々の古道具店「日々雑貨（hibi-zakka）」を、Klee One+ BIZ UDPMincho+ Yuji Syuku の3書体構成、生成り×麻色×抹茶×朱の配色、マスキングテープ ・ ポラロイド ・ はんこ ・ ノート罫線で構築する。

### Non-Goals
- 在庫検索、カート、決済
- 動画コンテンツ
- 海外フォントの併用

### Background
- 日々雑貨（hibi-zakka）は本デザイン研究のための架空店舗
- 同シリーズ4作の中で「親しみ ・ 手書き感」が最も強いサイト

---

## 2. User Stories

| ID | Persona | Want to | So that |
|----|---------|---------|---------|
| US-01 | 手しごと好きアパレル店員（三浦 結衣想定） | ページを開いた瞬間「友人に教わるおすすめ店」のような親しみを感じたい | 構えずに商品を覗ける |
| US-02 | 同上 | 店主の手帖（読みもの）を読みたい | 商品の選び方や手入れ方法を学べる |
| US-03 | 同上 | 催し（イベント）情報を知りたい | 蚤の市やワークショップに参加できる |
| US-04 | 同上 | スマホでも読みやすい配置がほしい | 通勤中にも気軽に覗ける |

### Acceptance Criteria

**US-01:**
- WHEN ページが読み込まれた THEN ポラロイド3枚＋手紙風の挨拶＋蛍光ペン風マーカーが現れる
- WHEN 表示後 THEN クラフト紙のような暖色背景と手書き書体が「友人の手紙」感を出す

**US-02:**
- WHEN 「店主の手帖」セクションに到達した THEN ノート罫線背景＋蛍光ペンの強調＋店主印で、エッセイ風に読める

**US-03:**
- WHEN 「催し」セクションに到達した THEN 月・日・曜日のカード＋会場情報＋参加費が確認できる

**US-04:**
- WHEN 375px 幅で閲覧した THEN ポラロイドは縦並びに、商品グリッドは1カラムに自動再配置される

---

## 3. Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-01 | ヒーロー（手紙風＋ポラロイド3枚） | P0 | 微回転で「貼った」感 |
| FR-02 | お店の品グリッド（6商品） | P0 | マスキングテープ装飾 |
| FR-03 | お便り（読みもの3本） | P0 | レターパッド風 |
| FR-04 | 催しセクション（4件） | P0 | 月・日カード |
| FR-05 | 店主の手帖（ノート罫線背景） | P0 | 蛍光ペンの強調 |
| FR-06 | 店の案内＋SNS リンク | P0 | 抹茶色のカードで強調 |
| FR-07 | スクロールリビール（float-in） | P1 | 15px、1s |
| FR-08 | ヘッダー（はんこ風シール） | P0 | 朱の丸シール |
| FR-09 | モバイル対応（375px） | P0 | ポラロイド・グリッド再構成 |

---

## 4. Architecture

```
zakka/index.html
├── <header>                       # 「日」のはんこ＋ロゴ
├── <section class="hero">         # 手紙風挨拶＋ポラロイド3枚
├── <section id="shop">            # お店の品 6商品
├── <section id="otayori">         # お便り 3本
├── <section id="moyooshi">        # 催し 4件
├── <section id="techo">           # 店主の手帖（ノート罫線）
├── <section class="annai">        # 店の案内＋SNS
└── <footer>                       # 4店舗ナビ
```

### Key Design Decisions

| Decision | Chosen | Rationale | Rejected |
|----------|--------|-----------|----------|
| テーマ | ライト（明） | クラフト紙の親しみ | ダーク・暖（民藝と被る） |
| 主書体 | Klee One | 教科書体の手書き感 | Hina Mincho（強すぎる） |
| 装飾 | テープ ・ ポラロイド ・ はんこ | 手紙の語彙 | 印章 ・ レコード |
| アニメ | float-in 軽快 | 紙が舞う感じ | 強いスライド |
| 補色 | 朱＋抹茶＋薄紅 | 和雑貨配色 | くすみカラー |

---

## 5. Design System

### Color Palette
```css
--kinari:#f7f1e8;
--asa:#d4c3a5;
--matcha:#697764;
--matcha-koi:#4a5c3a;
--shu:#c4513a;
--benibara:#d9a89d;
--kraft:#c8b48e;
--sumi:#2a2520;
```

### Typography
```css
--font-tegaki:'Klee One','Sawarabi Mincho',serif;
--font-mei:'BIZ UDPMincho','Sawarabi Mincho',serif;
--font-yu:'Yuji Syuku','Sawarabi Mincho',serif;
```

### Decoration
- マスキングテープ: `repeating-linear-gradient` で縞模様、`mix-blend-mode: multiply`
- ポラロイド: 白背景＋下にキャプション、`box-shadow` で軽い影
- ノート罫線: `repeating-linear-gradient(0deg, transparent 0, transparent 38px, ... 39px)`
- はんこ印: 円形＋朱、店主のサイン代わり

---

## 9. Testing Strategy

| Layer | Scenarios |
|-------|-----------|
| Desktop (1280px) | ポラロイド3枚の位置、グリッド6商品、ノート罫線連続 |
| Mobile (375px) | ポラロイド縦並び、グリッド1カラム化、催しカード再配置 |
| アニメ | float-in × IntersectionObserver なしの単純表示 |
| ホバー | 商品カード hover で transform translate＋shadow 強化 |
| フォント | Klee One ・ BIZ UDPMincho ・ Yuji Syuku 正常表示 |

---

## 10. Implementation Notes

- **ポラロイド回転**: 各カードに -5deg / +4deg / -2deg を割り当て、自然な「貼った」位置感を表現
- **テープ装飾**: `:nth-child(3n+1/2/n)` で3色のテープを順に配色（朱／抹茶／薄紅）
- **蛍光ペン**: `background: linear-gradient(transparent 60%, var(--benibara) 60%, ... 90%, transparent 90%)`
- **ノート罫線**: `repeating-linear-gradient` を section に重ね、左に1本の朱線で「左閉じノート」を再現
- **店主印**: 朱の円＋「印」文字、`::before` で書体を表現

---

## 11. Open Questions

| # | Question | Status |
|---|----------|--------|
| 1 | お便り（読みもの）を個別ページとして展開するか | Open |
| 2 | 催しは Google Calendar 連携するか | Open |
| 3 | SNS リンクを実装する場合の対象 SNS は | Confirmed: Instagram, Twitter, note |

---

## References

- [navigator README](../README.md)
- Font: [Klee One](https://fonts.google.com/specimen/Klee+One), [BIZ UDPMincho](https://fonts.google.com/specimen/BIZ+UDPMincho), [Yuji Syuku](https://fonts.google.com/specimen/Yuji+Syuku)
- Inspiration: 雑誌「天然生活」、雑誌「クウネル」、倉敷意匠の店構え、D&DEPARTMENT 和モダン側
