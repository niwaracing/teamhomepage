# NIWA RACING - 車両管理スキル定義書 (SKILLS.md)

本ドキュメントは、`sales.html` における在庫車両（Stock List）の追加、売却済み（SOLD OUT）更新、スペック変更などの保守作業を定型コマンド（スキル）化し、HTML構造・Tailwind CSSデザインシステム・Git運用ルールを厳格に維持しながら自律的・安定的に実行するための定義書です。

---

## 共通仕様・設計原則

1. **見切れ防止画像スタイル（アスペクト比維持）**:
   - 車両カードおよびモーダル内の画像表示枠には、必ず `aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative` を指定します。
   - `<img>` タグには `w-full h-full object-contain` を指定し、縦横比を崩さず画像全体が美しく収まるようにします（`object-cover` による不要なトリミングを防止）。
2. **在庫リストの並び順ルール**:
   - **販売中（In Stock）車両**: リストの上部（先頭〜中盤）に配置。新着車両は販売中リストの先頭に挿入します。
   - **売却済み（SOLD OUT）車両**: リストの最後尾にまとめて配置します。
3. **モーダル連動関数 `openCarModal` の引数構成**:
   ```javascript
   openCarModal(name, price, mileage, mission, inspection, imgSrc, isSoldOut = false, equipment = null, additionalImages = null)
   ```
   - 第1引数: 車種名（String）
   - 第2引数: モーダル用価格表記（String / 例: `'300万円（税込）'`, `'ASK'`）
   - 第3引数: 走行距離（String / 例: `'52,274km'`）
   - 第4引数: ミッション（String / 例: `'6速MT'`, `'AT'`）
   - 第5引数: 車検（String / 例: `'令和9年3月'`, `'2年付き'`, `'なし'`）
   - 第6引数: 代表画像パス（String / 例: `'images/MX-5/MX-5.jpg'`）
   - 第7引数: SOLD OUT判定フラグ（Boolean / `false` または `true`）
   - 第8引数: 装備・状態テキスト（Stringまたはnull / 省略可能）
   - 第9引数: 追加写真配列（Arrayまたはnull / 省略可能。文字列パスの配列、または `{src, alt}` のオブジェクト配列）

---

## １．add_vehicle（車両追加スキル）

新規の在庫車両を `sales.html` に追加し、関連ファイルとGitリポジトリを更新するスキル。

### 1.1 入力パラメータ仕様

| パラメータ名 | 必須/任意 | 型 | 説明・入力例 |
| :--- | :---: | :--- | :--- |
| `vehicle_name` | 必須 | String | 車種名（例: `マツダ ロードスター NR-A`） |
| `mileage` | 必須 | String | 走行距離（例: `38,000km`, `＊＊＊`） |
| `transmission` | 必須 | String | トランスミッション（例: `6速MT`, `AT`） |
| `inspection` | 必須 | String | 車検情報（例: `令和9年10月`, `2年付き`, `なし`） |
| `card_price` | 必須 | String | カード表示用価格（例: `¥ 2,480,000`, `ASK`） |
| `modal_price` | 必須 | String | モーダル表示用価格（例: `248万円（税込）`, `ASK`） |
| `main_image_path`| 必須 | String | メイン車両画像の配置パス（例: `images/roadster/roadster.jpg`） |
| `equipment` | 任意 | String | 装備や状態の詳細テキスト（例: `ロールケージ、ビルシュタイン車高調、機械式LSD`） |
| `additional_images`| 任意 | Array | 内装・外装等の追加写真パス配列（例: `['images/roadster/in1.jpg', 'images/roadster/out1.jpg']`） |

---

### 1.2 実行手順

1. **画像ディレクトリの作成とファイル配置**:
   - 単一画像の場合: `images/` 直下に配置（例: `images/235i.jpg`）または車種専用ディレクトリ `images/[車種フォルダ]/` を作成。
   - 複数写真（内装・外装ギャラリー）が存在する場合: 必ず `images/[車種名や型式]/` ディレクトリを作成し格納。
   - `SITE_STRUCTURE.md` の画像ツリーを更新（必要に応じて）。
2. **車両カードHTMLの生成**:
   - 後述の [1.3 HTML/Tailwind CSS テンプレート構造](#13-htmltailwind-css-テンプレート構造) に従ってHTMLブロックを生成。
   - 見切れ防止スタイル（`aspect-video bg-zinc-100` + `object-contain`）を厳格に適用。
3. **`sales.html` の在庫リストへの挿入**:
   - `<section id="stock-list">` 内の `<div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">` 直下を探す。
   - **販売中リストの先頭（1番目の車両の上）** に挿入（または指定された販売中車両グループ内の指定位置）。
   - ※売却済み（SOLD OUT）車両群より前に配置すること。
4. **構文およびスタイル確認**:
   - HTMLタグの閉じ漏れ、onclickのクォートエスケープミスがないか確認。
5. **Gitコミット＆Push**:
   - コミットメッセージ例:
     - `Add [車種名] to sales page and update stock list`
     - ギャラリー含む場合: `Add [車種名] with photo gallery to sales.html`
   - `git add .` → `git commit -m "..."` → `git push origin main`

---

### 1.3 HTML/Tailwind CSS テンプレート構造

#### パターンA: 基本構成（メイン画像のみ）

```html
<!-- {vehicle_name} -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm card-hover cursor-pointer" onclick="openCarModal('{vehicle_name}', '{modal_price}', '{mileage}', '{transmission}', '{inspection}', '{main_image_path}')">
    <div class="aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative">
        <img src="{main_image_path}" alt="{vehicle_name}" class="w-full h-full object-contain">
    </div>
    <div class="p-6">
        <div class="flex justify-between items-start mb-2">
            <h4 class="text-lg font-bold text-zinc-900">{vehicle_name}</h4>
        </div>
        <div class="text-sm text-zinc-500 space-y-1 mb-4">
            <p>走行: {mileage}</p>
            <p>ミッション: {transmission}</p>
            <p>車検: {inspection}</p>
        </div>
        <div class="text-xl font-bold text-rose-600 font-oswald">
            {card_price}
        </div>
    </div>
</div>
```

#### パターンB: フル構成（装備詳細・追加ギャラリー写真付き）

```html
<!-- {vehicle_name} -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm card-hover cursor-pointer" onclick="openCarModal('{vehicle_name}', '{modal_price}', '{mileage}', '{transmission}', '{inspection}', '{main_image_path}', false, '{equipment}', [{additional_images_json}])">
    <div class="aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative">
        <img src="{main_image_path}" alt="{vehicle_name}" class="w-full h-full object-contain">
    </div>
    <div class="p-6">
        <div class="flex justify-between items-start mb-2">
            <h4 class="text-lg font-bold text-zinc-900">{vehicle_name}</h4>
        </div>
        <div class="text-sm text-zinc-500 space-y-1 mb-4">
            <p>走行: {mileage}</p>
            <p>ミッション: {transmission}</p>
            <p>車検: {inspection}</p>
        </div>
        <div class="text-xl font-bold text-rose-600 font-oswald">
            {card_price}
        </div>
    </div>
</div>
```

※ `{additional_images_json}` の記述例:
`['images/demo/in1.jpg', 'images/demo/out1.jpg']` または
`[{src: 'images/demo/in1.jpg', alt: '内装'}, {src: 'images/demo/out1.jpg', alt: '外装'}]`

---

## ２．mark_sold_out（売却済み更新スキル）

成約した車両に対して SOLD OUT の視覚効果を付与し、在庫リストの最後尾へ並び替えるスキル。

### 2.1 入力パラメータ仕様

| パラメータ名 | 必須/任意 | 型 | 説明・入力例 |
| :--- | :---: | :--- | :--- |
| `target_vehicle_name` | 必須 | String | 売約済みにする対象車両の名前（例: `スズキ エブリイ`, `BMW 235i`） |

---

### 2.2 実行手順

1. **対象車両カードの特定**:
   - `sales.html` の `#stock-list` グリッド内から、`alt="{target_vehicle_name}"` または `<h4>{target_vehicle_name}</h4>` を含む対象の `<div>` カードブロックを特定。
2. **モーダル呼び出し引数の更新**:
   - `onclick="openCarModal(...)"` の第7引数（`isSoldOut`）を `true` に変更。
   - 例: `openCarModal('スズキ エブリイ', '39.8万円（税込）', '78,234km', 'MT', '令和8年10月', 'images/every.jpg', true)`
3. **SOLD OUT オーバーレイの追加**:
   - 画像コンテナ内に以下の子要素を追加：
     ```html
     <div class="absolute inset-0 bg-red-600/70 flex items-center justify-center text-white font-bold text-2xl tracking-widest z-10">SOLD OUT</div>
     ```
4. **価格表示の打ち消し線＆バッジ追加**:
   - 価格コンテナのクラスを `text-rose-600` から `text-zinc-400 line-through` に変更。
   - 価格テキストの直後に `<span class="ml-2 text-sm text-red-600 font-normal no-underline inline-block">売約済み</span>` を挿入。
5. **リスト最後尾への並び替え（リオーダー）**:
   - 更新したカードブロック全体を切り取り、`#stock-list` グリッドの**最下部（既存のSOLD OUT車両群の末尾）** へ移動。
   - これにより、販売中車両が常に上位に表示され、売約済み車両が下部に集約される。
6. **Gitコミット＆Push**:
   - コミットメッセージ例:
     - `Update [車種名] to SOLD OUT and reorder vehicle list by stock status`
   - `git add sales.html` → `git commit -m "..."` → `git push origin main`

---

### 2.3 HTML/Tailwind CSS テンプレート構造（Before / After）

#### 【Before】販売中状態
```html
<!-- スズキ エブリイ -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm card-hover cursor-pointer" onclick="openCarModal('スズキ エブリイ', '39.8万円（税込）', '78,234km', 'MT', '令和8年10月', 'images/every.jpg')">
    <div class="aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative">
        <img src="images/every.jpg" alt="スズキ エブリイ" class="w-full h-full object-contain">
    </div>
    <div class="p-6">
        <div class="flex justify-between items-start mb-2">
            <h4 class="text-lg font-bold text-zinc-900">スズキ エブリイ</h4>
        </div>
        <div class="text-sm text-zinc-500 space-y-1 mb-4">
            <p>走行: 78,234km</p>
            <p>ミッション: MT</p>
            <p>車検: 令和8年10月</p>
        </div>
        <div class="text-xl font-bold text-rose-600 font-oswald">
            ¥ 398,000
        </div>
    </div>
</div>
```

#### 【After】売約済み（SOLD OUT）状態
```html
<!-- スズキ エブリイ (SOLD OUT) -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm card-hover cursor-pointer" onclick="openCarModal('スズキ エブリイ', '39.8万円（税込）', '78,234km', 'MT', '令和8年10月', 'images/every.jpg', true)">
    <div class="aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative">
        <img src="images/every.jpg" alt="スズキ エブリイ" class="w-full h-full object-contain">
        <!-- SOLD OUT オーバーレイ -->
        <div class="absolute inset-0 bg-red-600/70 flex items-center justify-center text-white font-bold text-2xl tracking-widest z-10">SOLD OUT</div>
    </div>
    <div class="p-6">
        <div class="flex justify-between items-start mb-2">
            <h4 class="text-lg font-bold text-zinc-900">スズキ エブリイ</h4>
        </div>
        <div class="text-sm text-zinc-500 space-y-1 mb-4">
            <p>走行: 78,234km</p>
            <p>ミッション: MT</p>
            <p>車検: 令和8年10月</p>
        </div>
        <!-- 打ち消し線 ＋ 売約済みバッジ -->
        <div class="text-xl font-bold font-oswald text-zinc-400 line-through">
            ¥ 398,000<span class="ml-2 text-sm text-red-600 font-normal no-underline inline-block">売約済み</span>
        </div>
    </div>
</div>
```

---

## ３．モーダル側の自動制御（仕組み解説）

カード側の `openCarModal(..., isSoldOut = true)` が実行された際、`sales.html` 内部のJavaScriptによりモーダル側では以下の処理が自動で行われます：

1. **価格コンテナ**: 赤色文字（`text-rose-600`）からグレー＋打ち消し線（`text-zinc-400 line-through`）に切り替え。
2. **SOLD OUTオーバーレイ**: モーダル内メイン画像の上に `SOLD OUT` （文字サイズ: `text-4xl`）が表示される。
3. **売約済みバッジ**: 価格横の「売約済み」表示（`modalSoldOutBadge`）の `hidden` クラスが除去され表示される。
4. **問い合わせボタン**: 「この車両について問い合わせる」ボタン（`modalContactBtn`）に `hidden` クラスが付与され非表示化される。

---

## ４．保守作業チェックリスト（AIエージェント実行用）

作業完了前には以下の点を確認してください：
- [ ] 車両画像のコンテナクラスが `aspect-video bg-zinc-100 flex items-center justify-center overflow-hidden relative` になっているか
- [ ] `<img>` タグのクラスが `w-full h-full object-contain` になっているか（見切れが発生していないか）
- [ ] 販売中車両がリスト前半、SOLD OUT車両がリスト末尾にグループ分けされているか
- [ ] モーダル呼び出しの `openCarModal` の引数順序・型に狂いがないか
- [ ] `git status` / `git diff` で差分を確認し、意図しない箇所の変更がないか
- [ ] `AGENTS.md` の運用ポリシーに従い、作業後は自律的にGitコミット＆Pushを実施しているか
