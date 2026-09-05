# NIWA RACING Webサイト 構造・設計仕様書 (SITE_STRUCTURE.md)

本書は、NIWA RACING 公式Webサイト（[niwaracing/teamhomepage](https://github.com/niwaracing/teamhomepage.git)）のHTML構造、CSSスタイリング規則、共通UIパーツ、アセット配置ルール、および保守・開発ガイドラインをまとめた技術仕様書です。

---

## 1. サイト構成・ページ一覧

全ページが静的HTMLで構築されており、各ページはルートディレクトリ直下に配置されています。

| ファイル名 | タイトル / 役割 | 主なコンテンツ・概要 |
| :--- | :--- | :--- |
| [`index.html`](./index.html) | トップページ | ヒーロー（キャッチコピー）、コンセプト（Virtual to Real）、事業紹介（7大サービス）、参戦カテゴリー一覧、ドライバー・体制紹介、実績、問い合わせ・アクセス |
| [`categories.html`](./categories.html) | 参戦カテゴリー一覧 | 参戦中の全レースカテゴリーおよび参戦使用車両一覧 |
| [`rental.html`](./rental.html) | レーシングカーレンタル | レンタル車両ラインナップ（MX-5, NDロードスター, N-ONE, デミオ等）、利用料金、ご利用の流れ、規約、予約CTA |
| [`maintenance.html`](./maintenance.html) | メンテナンス・車両製作 | レース車両の点検・整備、セッティング、車両製作・チューニング、工賃表 |
| [`simulator.html`](./simulator.html) | シミュレーターレッスン | プロ仕様シミュレーター（iRacing等）によるドライビングレッスン、料金プラン、指導方針 |
| [`sales.html`](./sales.html) | 自動車販売・買取 | 新車・中古車販売、レース専用競技車両販売、カスタム車両製作、高価買取査定、在庫車両紹介 |
| [`coaching.html`](./coaching.html) | レーシングコーチング | データロガー（AIM/MoTeC等）解析、車載映像分析、同乗・先導走行レッスン、オンライン指導 |
| [`Stai.html`](./Stai.html) | スーパー耐久特設ページ | スーパー耐久シリーズ参戦情報、チームストーリー、参戦車両、ドライバー紹介、公式レースレポート（PDF閲覧＆アーカイブモーダル） |
| [`RPR.html`](./RPR.html) | ロードスター・パーティレース | ROADSTER Party Race 参戦情報 |
| [`Fuji.html`](./Fuji.html) | 富士チャンピオンレース | Fuji Champion Race 参戦情報 |
| [`Nonecup.html`](./Nonecup.html) | N-ONE OWNER'S CUP | N-ONE ワンメイクレース参戦情報 |
| [`yaris.html`](./yaris.html) | Yaris Cup | TOYOTA GAZOO Racing Yaris Cup 参戦情報 |
| [`mazda-fan.html`](./mazda-fan.html) | マツ耐 | MAZDA Fan ENDURANCE 参戦情報 |
| [`joytai.html`](./joytai.html) | JOY耐 | もてぎEnjoy耐久レース 参戦情報 |
| [`starfield.html`](./starfield.html) | Starfield idlers Games | アイドラーズ・ゲームス参戦情報 |

---

## 2. HTML基本構造・外部リソース仕様

すべてのHTMLファイルは以下の共通構造・CDN定義に準拠して記述されています。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>【ページ名】 - NIWA RACING</title>
    
    <!-- CSSフレームワーク: Tailwind CSS (CDN) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- アイコンフォント: FontAwesome 6 (CDN) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <!-- 共通スタイル定義 -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Oswald:wght@400;700&family=Noto+Sans+JP:wght@400;700&display=swap');
        
        body {
            font-family: 'Noto Sans JP', sans-serif;
            background-color: #ffffff;
            color: #1a1a1a;
            scroll-behavior: smooth;
        }
        .font-oswald {
            font-family: 'Oswald', sans-serif;
        }
        .hero-gradient {
            background: linear-gradient(rgba(255,255,255,0.1), rgba(255,255,255,1)), url('./images/car-main.jpg');
            background-size: cover;
            background-position: center;
        }
        .accent-border {
            border-left: 4px solid #e11d48;
        }
        .text-glow {
            text-shadow: 0 0 10px rgba(225, 29, 72, 0.3);
        }
        .img-placeholder {
            background-color: #f4f4f5;
        }
        /* カスタムスクロールバー */
        .custom-scrollbar::-webkit-scrollbar {
            width: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.05);
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #e11d48;
            border-radius: 10px;
        }
        .nav-link:hover {
            color: #e11d48;
        }
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px -10px rgba(0,0,0,0.1);
        }
        .footer-map iframe {
            filter: grayscale(0.1) contrast(1.1);
            border-radius: 1rem;
        }
    </style>
</head>
<body class="antialiased">
```

---

## 3. CSS・デザインシステム規則

### 3.1 カラーパレット
- **アクセント / ブランドカラー**: `#e11d48`（Rose 600）
  - Tailwindクラス: `text-rose-600`, `bg-rose-600`, `hover:bg-rose-700`, `border-rose-600`
  - ボタン、強調見出し、アクセントライン、アイコン等に使用。
- **ベース背景色**: `#ffffff`（白）、セクション区切り用: `bg-zinc-50` または `bg-gray-50`
- **ダークセクション背景色**: `bg-zinc-950` / `bg-black/60`（ストーリー・ヒーローセクション等）
- **文字色**:
  - 主要テキスト: `#1a1a1a` / `text-zinc-900`
  - サブテキスト・説明文: `text-zinc-600`, `text-zinc-500`, `text-zinc-400`
  - ダーク背景上のテキスト: `text-white`, `text-gray-200`
- **ボーダー**: `border-gray-100`, `border-zinc-200`

### 3.2 タイポグラフィ
- **日本語本文・標準テキスト**: `'Noto Sans JP', sans-serif`（ウェイト: 400 / 700）
- **英字見出し・ブランドキーワード**: `'Oswald', sans-serif`（クラス名: `.font-oswald`、ウェイト: 400 / 700、`uppercase tracking-wider` と併用）
- **ストーリー・縦書き強調部（一部）**: 明朝体（`font-mincho`）

### 3.3 コンポーネント用カスタムCSSクラス
| クラス名 | 定義内容 | 用途 |
| :--- | :--- | :--- |
| `.font-oswald` | `font-family: 'Oswald', sans-serif;` | 英字見出し・サブタイトル |
| `.accent-border` | `border-left: 4px solid #e11d48;` | セクション見出しの左アクセント線 |
| `.card-hover` | `transform: translateY(-5px); box-shadow: ...` | カード要素のマウスホバー時の浮き上がり演出 |
| `.nav-link` | `transition` と併用しホバー時に `#e11d48` | ナビゲーションリンク |
| `.hero-gradient` | リニアグラデーション ＋ メイン背景画像 | トップページやヘッダーの背景 |
| `.footer-map iframe` | `filter: grayscale(0.1) contrast(1.1); border-radius: 1rem;` | フッターのGoogleマップ埋め込みスタイル |

---

## 4. 共通UIパーツ仕様

### 4.1 固定ヘッダー & ナビゲーション (`<nav>`)
全ページ共通で画面上部に固定（`fixed w-full z-50 bg-white border-b border-gray-100 shadow-sm`）。

- **ロゴ**: `./images/teamrogo.jpg`（高さ `h-14 w-auto object-contain`）
- **ナビゲーションメニュー項目（主要ページ構成）**:
  1. ホーム（[`index.html`](./index.html)）
  2. 参戦カテゴリー（[`categories.html`](./categories.html)）
  3. スーパー耐久特設（[`Stai.html`](./Stai.html)）※主要ページにて追加
  4. レーシングカーレンタル（[`rental.html`](./rental.html)）
  5. メンテナンス（[`maintenance.html`](./maintenance.html)）
  6. レーシングシミュレーターレッスン（[`simulator.html`](./simulator.html)）
  7. 自動車販売・買取（[`sales.html`](./sales.html)）
  8. コーチング（[`coaching.html`](./coaching.html)）
- **レスポンシブ挙動**:
  - PC（`lg:block`）: 横並びメニュー（`text-sm font-bold whitespace-nowrap`）
  - モバイル（`lg:hidden`）: ハンバーガーボタン（`#mobile-menu-btn`）とトグル式ドロップダウン（`#mobile-menu`）
- **モバイルメニュースクリプト**:
  ```javascript
  document.addEventListener('DOMContentLoaded', function() {
      const btn = document.getElementById('mobile-menu-btn');
      const menu = document.getElementById('mobile-menu');
      if (btn && menu) {
          btn.addEventListener('click', () => {
              menu.classList.toggle('hidden');
          });
      }
  });
  ```

### 4.2 フッター (`<footer id="contact">`)
全ページ共通で末尾に配置（`bg-white py-24 px-4 border-t border-gray-100`）。

- **4カラムレイアウト（PC時）**:
  - **Column 1**: チームロゴ、所在地、電話番号、TEL/FAX、メールアドレス、SNSリンク（X/Twitter, Instagram, Facebook）
  - **Column 2〜4**: Google Maps iframe（長野県上田市国分1530-1）
- **下部コピーライト**:
  - `© 2025 NIWA RACING TEAM. All Rights Reserved.`
  - プライバシーポリシー / 利用規約リンク

### 4.3 コンタクト情報（統一表記）
- **所在地**: 長野県上田市国分1530-1
- **携帯番号**: 080-1172-4441
- **TEL＆FAX**: 0268-84-2674
- **E-mail**: `niwaracing@gmail.com`
- **SNSアカウント**:
  - X (Twitter): `https://x.com/niwaracing` (`@niwaracing`)
  - Instagram: `https://www.instagram.com/niwaracing` (`@niwaracing`)
  - Facebook: `https://www.facebook.com/eiji.niwa.5/` (`NIWA RACING`)

### 4.4 レースレポートセクション・アーカイブモーダル（`Stai.html` 特設）
スーパー耐久特設ページ（`Stai.html`）専用の戦績報告コンポーネント。
- **配置位置**: `<section id="gallery-team">`（Gallery & Team）と `<section id="sponsor-cta">`（スポンサー募集）の間。
- **メイン画面表示（最新レース1件表示）**:
  - ページ肥大化を防ぎ最新情報を素早く届けるため、直近の最新レース1件（Round 5 オートポリス戦）のみを中央にフィーチャーカードとして表示。
  - 大会名、日程（カレンダーアイコン付き）、サーキット名（マップピンスタイル）、別タブで開く「最新レポートを読む (PDF)」ボタン（`target="_blank" rel="noopener noreferrer"`）。
  - 下部に過去の全レポート閲覧モーダルを起動するボタン（`#open-report-modal`）を配置。
- **アーカイブモーダル（`#report-modal`）**:
  - 全シーズン・全ラウンドのレポートPDFを最新順（降順：Rd.5 → Rd.4 → Rd.3 → Rd.1）で一覧できるポップアップモーダル。
  - シーズン（2026 SEASON等）ごとに見出しを設け、整然としたリスト形式で表示。
  - 背景オーバーレイ（ぼかし＋暗転）、ESCキー押下・背景クリック・閉じるボタンによるスムーズなアニメーション開閉。
  - モーダル表示中は背景のスクロールを固定（`overflow-hidden`）。

---

## 5. アセット（画像・ドキュメント・スクリプト）配置規則

### 5.1 ディレクトリ構成
```text
reports/                  # レースレポートPDFディレクトリ
├── 2026_rd1_motegi.pdf   # 2026 Rd.1 もてぎ戦 レースレポート (PDF)
├── 2026_rd3_fuji24h.pdf  # 2026 Rd.3 富士24時間戦 レースレポート (PDF)
├── 2026_rd4_sugo.pdf     # 2026 Rd.4 SUGO 4時間戦 レースレポート (PDF)
└── 2026_rd5_autopolis.pdf# 2026 Rd.5 オートポリス戦 レースレポート (PDF)

images/
├── teamrogo.jpg          # メインチームロゴ
├── nwia.jpg / nwia.webp  # ロゴバリエーション
├── car-main.jpg          # ヒーロー用メインビジュアル
├── mx5.jpg               # GLOBAL MX-5 CUP 車両
├── car-roadstar.jpg      # ロードスター（ND5RC）
├── car-none.jpg          # N-ONE レース車両
├── AQUA.jpg              # アクア レース車両
├── 235i.jpg              # BMW 235i
├── 250.jpg               # レース車両画像
├── Megane.jpg            # ルノー メガーヌRS
├── GR86cup.jpg           # GR86 Cup
├── car-ST5R.jpg          # スーパー耐久 ST-5 車両
├── 288_1.jpg〜288_3.jpg   # スーパー耐久参戦・車両画像
├── shugo1.jpg〜shugo3.jpg # チーム集合写真
├── driver1.jpg〜driver3.jpg # ドライバー写真
├── meca1.jpg, meca2.jpg  # メカニック・整備風景
├── sim.jpg               # シミュレーター設備
├── coaching1.jpg         # コーチング風景
├── logger1.jpg, logger2.jpg # ロガー・データ解析
├── movie.jpg             # 車載・動画撮影
├── demio/                # デミオ専用ディレクトリ
│   ├── demio.jpg
│   ├── in1.jpg, in2.jpg, in3.jpg   # 内装・ロールケージ写真
│   └── out1.jpg, out2.jpg          # 外装写真
└── MX-5/                 # MX-5 CUPカー専用ディレクトリ
    ├── MX-5.jpg          # メイン車両写真
    ├── MX-5in1.jpg, MX-5in2.jpg # 内装写真
    └── MX-5out1.jpg, MX-5out2.jpg # 外装写真
```

### 5.2 画像利用のルール
- 相対パスは原則 `./images/[ファイル名]` または `./images/[カテゴリ]/[ファイル名]` で指定する。
- `<img>` タグには適切な `alt` 属性（例: `alt="GLOBAL MX-5 CUP"`）を必ず付与する。
- アスペクト比固定・トリミングには Tailwind の `object-cover`、`object-contain`、`h-48 w-full` 等を使用する。

---

## 6. 開発・保守ガイドライン（エージェント運用規則）

1. **共通ナビゲーション・フッターの整合性維持**:
   - 新規ページの追加や既存ページのナビゲーション変更を行う際は、全HTMLファイルの `<nav>` および `<footer>` の表記・リンク先・IDを完全に同期させること。
2. **デザインシステムの踏襲**:
   - スタイリングは Tailwind CSS クラスを基本とし、カラーは Rose（`#e11d48`）をアクセントとして統一すること。
   - 英字見出しには `.font-oswald` を適用すること。
3. **Git運用ポリシー（[AGENTS.md](./AGENTS.md) 準拠）**:
   - HTML/CSSの生成・更新完了後は、自律的にGitコミットを作成し、必要に応じてPushを行うこと。
   - ファイルの完全削除やリポジトリの履歴改変を行う場合は、事前にユーザーの承認を得ること。
