---
name: slide-creator
description: "お客さん向けのスライド資料（営業・技術解説・セキュリティ説明など）をHTMLで作成するスキル。「スライドを作って」「資料を作りたい」「プレゼン資料」「営業資料」「解説資料」「セキュリティの説明資料」などの依頼が来たときに必ず使用する。口頭で内容を受け取り、構成案を提示して確認を取ってからHTMLスライドを生成する。スライド・発表資料・説明資料に関する依頼であれば、言い回しが違っても必ずこのスキルを使用する。"
---

# スライド作成アシスタント

お客さん向けスライド資料をHTMLで生成するスキル。
口頭入力 → 構成確認 → スタイル選択 → HTML生成 の4ステップで動作する。

出力したHTMLはそのままブラウザで表示して使用する（HTML表示が最終成果物）。PowerPointへの貼り付けは前提としない。
`.pptx` 直接生成は行わない（HTML表示を主用途とするため）。

## 全体フロー

```
[Step 1] ユーザーから内容を口頭で受け取る
  → テーマ・目的・対象・伝えたいことを読み取る
  → 不明点があれば最大2問・1回にまとめて確認する（聞きすぎない）
  → 対象読者が明示されていない場合は必ず確認する

[Step 2] 構成案（アジェンダ）を提示する
  → スライドタイトル一覧 + 各スライドの概要1行 を箇条書きで提示
  → 「この構成で進めてよいですか？変更があればお知らせください。」と確認する
  → 修正があれば対応し、再確認する

[Step 3] スタイルを選ばせる
  → 構成OKを受けてからスタイル選択を提示する
  → ユーザーが最初からスタイルを指定している場合はスキップする

[Step 4] HTMLスライドを生成する
  → 1HTMLファイルにすべてのスライドを収める
  → present_files で提示する
  → 修正依頼があれば対応する
```

## Step 1 — 内容の読み取り

以下を会話から読み取る。明示されていなければ文脈から推定してよい。

| 項目             | 読み取り方                                 |
| ---------------- | ------------------------------------------ |
| テーマ・タイトル | 話の中心になるキーワードから推定           |
| 目的             | 営業（売り込み）か解説（理解促進）かを判断 |
| 対象読者         | IT担当者か経営層か一般社員か               |
| 伝えたいこと     | 課題・解決策・メリット・手順のどれが主軸か |
| 枚数の目安       | デフォルト5〜8枚。指定があればそれに従う   |

不明点は最大2問まで。質問は1回にまとめて行う。

## Step 2 — 構成案の提示

以下のフォーマットで提示する：

```
## スライド構成案

1. **タイトルスライド** — [資料タイトル・会社名など]
2. **[スライドタイトル]** — [このスライドで伝えること1行]
3. **[スライドタイトル]** — [このスライドで伝えること1行]
...
N. **クロージング / まとめ** — [次のアクションや結論]

この構成で進めてよいですか？変更があればお知らせください。
```

構成を決めるときの原則：

- 1枚に伝えることは1つ
- 「問題提起 → 解決策 → 根拠 → まとめ」の流れを基本とする
- 技術解説の場合は「概要 → 仕組み → リスク・影響 → 対策 → まとめ」を使う
- タイトルスライドとまとめスライドは必ず含める

## Step 3 — スタイル選択

構成確認後、以下を提示してスタイルを選ばせる：

```
スタイルを選んでください：

A. Apple Minimal — 白ベース・細線・余白重視のモノクロ。落ち着いた印象。
B. Google系（Material Design） — 白ベース＋Googleの4色（青・赤・黄・緑）を要素ごとに使い分ける。大きめの角丸・やわらかい影で浮かぶカード・ピル形のチップ。明るくフレンドリーで、複数の要素を色で識別させたいときに。
C. Linear系 — ダーク基調・紫(藍)1色のアクセント・隅のオーロラ発光。静かに作り込まれた印象。テック／スタートアップ寄りの相手に。
D. Magenta Brand（自社系） — Apple Minimalにマゼンタ（#e4007f）をアクセントとして加えたスタイル。自社カラーで統一感を出したいときに。
```

ユーザーがスタイルを指定したら生成を開始する。

## Step 4 — HTML生成ルール

### 構造

- 1HTMLファイルにすべてのスライドを収める
- 各スライドは `<div class="slide">` で区切り、表示中のスライドに `active` クラスを付与する
- 表示制御は `.slide{display:none}` と `.slide.active{display:flex}` に一任する。レイアウト調整用のクラス（パディング・flex方向など）を `.slide` に併用する場合、そのクラスに `display` を指定しない。同詳細度で後勝ちすると非アクティブなスライドも表示され、全ページが縦に積み重なる
- スライドサイズは `aspect-ratio: 16 / 9` で維持し、`max-width: 1280px` で上限を設ける
- オフライン環境でも確実に表示できるよう、フォントはシステムフォントのみ使用する（Webフォント不使用。客先でネットが不通でも崩れないため）
- 日本語フォントは必ず明示する：`"Hiragino Sans", "Hiragino Kaku Gothic ProN", "Yu Gothic", sans-serif`
- 外部リソース（CDN・画像URL）は一切使用しない

### テキスト

| 要素                                          | font-size (clamp)           | font-weight | color                   |
| --------------------------------------------- | --------------------------- | ----------- | ----------------------- |
| タイトルスライドの主タイトル                  | `clamp(38px, 4.4vw, 56px)`  | 700         | `var(--text-primary)`   |
| セクション見出し（h2）                        | `clamp(22px, 2.4vw, 32px)`  | 700         | `var(--text-primary)`   |
| リード文（.lead）                             | `clamp(13px, 1.35vw, 17px)` | 400         | `var(--text-secondary)` |
| タグ・ピルラベル                              | `clamp(13px, 1.3vw, 16px)`  | 700         | スタイル依存            |
| カードタイトル                                | `clamp(18px, 1.8vw, 22px)`  | 700         | `var(--text-primary)`   |
| カード本文                                    | `clamp(16px, 1.6vw, 18px)`  | 400         | `var(--text-primary)`   |
| コンテナ内チップラベル（fig-chip / col-chip） | `clamp(14px, 1.4vw, 18px)`  | 700         | スタイル依存            |
| コンテナ内本文（fig-half p / col p / wall p） | `clamp(17px, 1.7vw, 21px)`  | 400         | `var(--text-primary)`   |
| 番号リスト見出し（num h3）                    | `clamp(18px, 1.8vw, 22px)`  | 700         | `var(--text-primary)`   |
| 番号リスト本文（num p）                       | `clamp(16px, 1.6vw, 20px)`  | 400         | `var(--text-primary)`   |
| チェックリスト本文（check-item p）            | `clamp(17px, 1.7vw, 21px)`  | 400         | `var(--text-primary)`   |
| キャプション・補足（.note）                   | `clamp(10px, 0.95vw, 12px)` | 400         | `var(--text-muted)`     |

- **本文色の原則**：カード本文・リストアイテム・説明文は `var(--text-primary)` を基本とする。`--text-secondary` はリード文・補足情報のみに限定する（スライドはモニター距離から見るため、薄いグレーは可読性が低い）
- line-height：本文 1.65〜1.75、リスト・チェックリスト 1.65
- 1スライドのテキスト量は少なめに保つ（箇条書きは4〜6項目まで）
- **フォントサイズの縮小判断**：上記サイズを適用した結果、テキストがコンテナからはみ出す・余白が極端に狭くなる・行数が多すぎてスライドが窮屈になる場合は、該当要素のフォントサイズを1段階（2〜4px）下げて調整する。無理に詰め込まず、必要であればスライドを分割することを優先する

### ビジュアル要素

- 図・アイコンはSVGインラインで描画する（`viewBox` を明示、`fill="none"` をデフォルトに）
- SVGの色はCSSで制御する。インライン属性の `fill="var(--xxx)"` / `stroke="var(--xxx)"` は多くのブラウザで効かないため使わない。`fill="currentColor"`（親要素に `color` を指定）か、CSSクラス（例：`.lbl-build{fill:var(--accent)}`）で着色する
- データや比較はテーブルまたはシンプルなSVGグラフで表現する
- テーブルのハイライトセル（選択中の行・列など）の枠は、`border-color` ではなく `box-shadow: inset 0 0 0 2px var(--accent)` で描く。`border-collapse: collapse` では隣接セルと枠線が衝突し、緑枠などが一部欠けて見えるため
- 装飾的なフルカラーバーや派手なグラデーションは使わない（例外：Linear系の「隅のオーロラ発光」／Google系の「やわらかいelevation影」。下記B・C参照）
- タイトル下のアクセントラインは使わない（AI感が出るため）
- アイコン背景には `border-radius: 50%` の円形ラップを使う（Linear系のみ角丸スクエア。下記C参照。Google系は円形のまま）
- SVGの表示サイズは HTML属性（`width` / `height`）ではなく CSS で制御してレスポンシブに対応する。`width: min(82%, 370px); height: auto;` のように `min()` で上限とコンテナ割合の小さい方を取る。スライド幅が変わっても図がはみ出さない

### スペーシング基準

| 場所                                     | 値                             |
| ---------------------------------------- | ------------------------------ |
| 縦並びスライドのパディング（`.pad`）     | `3% 4%`                        |
| 分割レイアウト（左右パネル）のパディング | `5% 5.5%`                      |
| コンテナ内パディング（fig-half / col）   | `20px 24〜26px`                |
| カードパディング                         | `20px 20px 18px`               |
| カードアイコン直径                       | `44px`（`border-radius: 50%`） |
| 番号バッジ直径                           | `36px`（`border-radius: 50%`） |
| カード間ギャップ                         | `14px`                         |
| タグとh2の間のmargin-top                 | `14〜18px`                     |
| リスト・カードグリッドのmargin-top       | `16〜18px`                     |

### ナビゲーション

```javascript
const slides = document.querySelectorAll('.slide');
let current = 0;

function changeSlide(dir) {
  slides[current].classList.remove('active');
  current = Math.max(0, Math.min(slides.length - 1, current + dir));
  slides[current].classList.add('active');
  // スライド番号表示・ボタンのdisabled状態も更新する
}

document.addEventListener('keydown', e => {
  if (epMode) return; // 編集モード中はキー操作を無効にする
  if (e.key === 'ArrowRight') changeSlide(1);
  if (e.key === 'ArrowLeft') changeSlide(-1);
});
```

- ナビゲーションボタン（← →）と編集トグルボタンは `.slide-footer` ラッパーで包む。ナビは中央、トグルはスライド右端に揃える（後述の「編集UI」の CSS・HTML 参照）
- スライド番号（例：3 / 7）はボタンの間に表示する
- 先頭・末尾では対応するボタンを `disabled` にする
- ボタンのhoverに `var(--accent)` 色を適用する


### 編集UI

すべての生成HTMLに以下の編集UIを含める。ブラウザ上でテキスト・書式を直接編集し、編集済みHTMLとして保存できる機能。

#### 動作概要

- スライド右下（ページ送りボタンと同列）の「編集モード OFF」ボタンでON/OFFを切り替える
- ONにすると右側にサイドパネルが開く。パネルの左端をドラッグして幅を調整できる
- パネル幅に連動してスライドエリアが左に縮小し、パネルにコンテンツが隠れない
- スライド内テキスト要素をクリックで選択し、テキスト内容・フォントサイズ・太字・文字色・配置を変更できる
- グローバル設定でアクセントカラーを一括変更できる（`--accent` と `--accent-light` を更新）
- 「↓ HTMLをダウンロード」で編集内容を反映したHTMLを保存する（編集モードOFF状態でシリアライズ）
- ダウンロードしたHTMLを再度ブラウザで開けば再編集可能

#### 配置

| パーツ     | 配置場所                           |
| ---------- | ---------------------------------- |
| CSS        | `<style>` タグ末尾に追記           |
| HTML       | `</body>` 直前                     |
| JavaScript | 既存のナビゲーション JS 末尾に追記 |

#### CSS

```css
/* ===== EDITOR UI ===== */
/* .slide-footer：ナビ（中央）＋編集トグル（スライド右端）を同一行に並べるラッパー */
.slide-footer {
  width: 100%; max-width: 1280px;
  padding: 12px 24px 4px; /* slide-wrapper と横 padding を揃える */
  box-sizing: border-box;
  display: flex; align-items: center; justify-content: center;
  position: relative;
}

#ep-toggle {
  /* .slide-footer 内で absolute 配置。right:24px = slide-wrapper の padding と同じ = スライド右端 */
  position: absolute; right: 24px;
  top: 50%; transform: translateY(-50%); /* ナビボタンと縦位置を揃える */
  z-index: 10;
  padding: 7px 14px;
  border: 1.5px solid var(--text-primary, #1d1d1b);
  border-radius: 6px;
  background: var(--bg, #fff);
  color: var(--text-primary, #1d1d1b);
  font-size: 12px; font-family: inherit; font-weight: 600;
  cursor: pointer; letter-spacing: 0.01em;
  transition: background 0.15s, color 0.15s;
  white-space: nowrap;
}
#ep-toggle.on {
  background: var(--text-primary, #1d1d1b);
  color: var(--bg, #fff);
}
#ep-panel {
  position: fixed; top: 0; right: 0; z-index: 2000;
  width: var(--ep-w, 256px);   /* 可変幅。ドラッグで --ep-w を更新 */
  min-width: 180px; max-width: 520px;
  height: 100vh;
  background: #ffffff;
  border-left: 1px solid #e0dfd8;
  box-shadow: -2px 0 12px rgba(0,0,0,0.1);
  overflow-y: auto;
  padding: 16px;
  box-sizing: border-box;
  display: none;
  font-family: -apple-system, system-ui, "Hiragino Sans", sans-serif;
  font-size: 13px; color: #1d1d1b;
}
#ep-panel.open { display: block; }

/* リサイズハンドル（パネル左端） */
#ep-resize {
  position: absolute; left: 0; top: 0;
  width: 5px; height: 100%;
  cursor: col-resize; z-index: 1;
  background: transparent;
}
#ep-resize:hover, body.ep-resizing #ep-resize { background: rgba(0,0,0,0.1); }

/* スライドエリアをパネル幅分だけ縮小 */
body.edit-mode { padding-right: var(--ep-w, 256px); }
body.ep-resizing { user-select: none; cursor: col-resize; }

.ep-head {
  font-size: 13px; font-weight: 700;
  padding-bottom: 12px; margin-bottom: 16px;
  border-bottom: 1px solid #e0dfd8;
}
.ep-sec { margin-bottom: 20px; }
.ep-sec-title {
  font-size: 10px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 0.1em;
  color: #aeaca4; margin-bottom: 10px;
}
.ep-row {
  display: flex; align-items: flex-start;
  gap: 8px; margin-bottom: 10px;
}
.ep-lbl {
  font-size: 12px; width: 60px;
  flex-shrink: 0; padding-top: 5px; line-height: 1.4;
}
.ep-unit { font-size: 12px; color: #86847c; padding-top: 6px; }
#ep-text {
  flex: 1;
  border: 1px solid #c8c6bc; border-radius: 4px;
  padding: 6px 8px; font-size: 12px; font-family: inherit;
  resize: none; overflow: hidden;   /* スクロールなし・自動伸縮 */
  line-height: 1.5; min-height: 28px; width: 100%;
}
#ep-size {
  width: 60px;
  border: 1px solid #c8c6bc; border-radius: 4px;
  padding: 5px 8px; font-size: 12px;
}
.ep-cpick {
  width: 36px; height: 30px; padding: 2px 3px;
  border: 1px solid #c8c6bc; border-radius: 4px;
  cursor: pointer; background: none;
}
.ep-btn, .ep-grp button {
  border: 1px solid #c8c6bc; border-radius: 4px;
  padding: 5px 10px; font-size: 12px; font-family: inherit;
  cursor: pointer; background: #fff; color: #1d1d1b;
  transition: all 0.1s;
}
.ep-grp { display: flex; gap: 4px; }
.ep-btn.on, .ep-grp button.on {
  background: #1d1d1b; color: #fff; border-color: #1d1d1b;
}
#ep-hint {
  font-size: 12px; color: #aeaca4;
  text-align: center; padding: 16px 0;
}
#ep-dl {
  width: 100%; padding: 10px; border: none;
  border-radius: 6px; background: #1d1d1b; color: #fff;
  font-size: 13px; font-family: inherit; font-weight: 600;
  cursor: pointer; margin-top: 8px; transition: opacity 0.15s;
}
#ep-dl:hover { opacity: 0.8; }
body.edit-mode [data-ep] { cursor: pointer; border-radius: 3px; }
body.edit-mode [data-ep]:hover {
  outline: 1.5px dashed var(--accent, #1d1d1b);
  outline-offset: 2px;
}
body.edit-mode [data-ep].ep-sel {
  outline: 2px solid var(--accent, #1d1d1b);
  outline-offset: 2px;
}
```

#### HTML

```html
<!-- フッター：ナビ（中央）＋編集トグル（スライド右端）を同一行に配置 -->
<!-- ナビゲーション HTML をこの div で包む（slide-footer が既存の slide-nav を置き換える） -->
<div class="slide-footer">
  <nav class="slide-nav">
    <button id="btn-prev" onclick="changeSlide(-1)" disabled>←</button>
    <span id="slide-num">1 / N</span>
    <button id="btn-next" onclick="changeSlide(1)">→</button>
  </nav>
  <button id="ep-toggle" onclick="toggleEP()">編集モード OFF</button>
</div>

<!-- EDITOR PANEL -->
<div id="ep-panel">
  <div id="ep-resize"></div><!-- リサイズハンドル -->
  <div class="ep-head">スライド編集</div>
  <div class="ep-sec">
    <div class="ep-sec-title">グローバル設定</div>
    <div class="ep-row">
      <span class="ep-lbl">アクセント色</span>
      <input type="color" class="ep-cpick" id="ep-accent" oninput="epAccent(this.value)">
    </div>
  </div>
  <div class="ep-sec">
    <div class="ep-sec-title">テキスト要素</div>
    <div id="ep-hint">テキストをクリックして選択</div>
    <div id="ep-ctrl" style="display:none">
      <div class="ep-row">
        <span class="ep-lbl">内容</span>
        <textarea id="ep-text" oninput="epText(this.value)"></textarea>
      </div>
      <div class="ep-row">
        <span class="ep-lbl">サイズ</span>
        <input type="number" id="ep-size" min="8" max="80" oninput="epSize(this.value)">
        <span class="ep-unit">px</span>
      </div>
      <div class="ep-row">
        <span class="ep-lbl">太字</span>
        <button class="ep-btn" id="ep-bold" onclick="epBold()">B</button>
      </div>
      <div class="ep-row">
        <span class="ep-lbl">文字色</span>
        <input type="color" class="ep-cpick" id="ep-color" oninput="epColor(this.value)">
      </div>
      <div class="ep-row">
        <span class="ep-lbl">配置</span>
        <div class="ep-grp">
          <button onclick="epAlign('left')" data-a="left">左</button>
          <button onclick="epAlign('center')" data-a="center">中</button>
          <button onclick="epAlign('right')" data-a="right">右</button>
        </div>
      </div>
    </div>
  </div>
  <button id="ep-dl" onclick="epDownload()">↓ HTMLをダウンロード</button>
</div>
```

#### JavaScript

```javascript
// ===== EDITOR =====
let epMode = false, epSel = null;
let epRsz = false, epRszX = 0, epRszW = 0; // リサイズ状態

window.addEventListener('DOMContentLoaded', () => {
  // アクセントカラーの初期値を読み込む
  const raw = getComputedStyle(document.documentElement).getPropertyValue('--accent').trim();
  const hex = epColorToHex(raw);
  if (hex) document.getElementById('ep-accent').value = hex;

  // スライド内テキスト要素に data-ep 属性を付与してクリックリスナーを登録
  document.querySelectorAll(
    '.slide h1,.slide h2,.slide h3,.slide p,.slide li,.slide td,.slide th'
  ).forEach((el, i) => {
    el.dataset.ep = i;
    el.addEventListener('click', e => {
      if (!epMode) return;
      e.stopPropagation();
      epPick(el);
    });
  });

  // リサイズハンドル
  const handle = document.getElementById('ep-resize');
  handle.addEventListener('mousedown', e => {
    epRsz = true;
    epRszX = e.clientX;
    epRszW = parseInt(getComputedStyle(document.getElementById('ep-panel')).width) || 256;
    document.body.classList.add('ep-resizing');
    e.preventDefault();
  });
  document.addEventListener('mousemove', e => {
    if (!epRsz) return;
    const w = Math.max(180, Math.min(520, epRszW + (epRszX - e.clientX)));
    document.documentElement.style.setProperty('--ep-w', w + 'px');
  });
  document.addEventListener('mouseup', () => {
    if (epRsz) { epRsz = false; document.body.classList.remove('ep-resizing'); }
  });
});

function toggleEP() {
  epMode = !epMode;
  document.getElementById('ep-toggle').textContent = epMode ? '編集モード ON' : '編集モード OFF';
  document.getElementById('ep-toggle').classList.toggle('on', epMode);
  document.getElementById('ep-panel').classList.toggle('open', epMode);
  document.body.classList.toggle('edit-mode', epMode);
  if (!epMode) epClear();
}

function epPick(el) {
  if (epSel) epSel.classList.remove('ep-sel');
  epSel = el;
  el.classList.add('ep-sel');
  document.getElementById('ep-hint').style.display = 'none';
  document.getElementById('ep-ctrl').style.display = 'block';

  document.getElementById('ep-text').value = el.innerText;
  epAutosize(); // テキストエリア高さを内容に合わせる
  document.getElementById('ep-size').value = Math.round(parseFloat(getComputedStyle(el).fontSize));
  document.getElementById('ep-bold').classList.toggle('on', parseInt(getComputedStyle(el).fontWeight) >= 600);
  const hex = epColorToHex(getComputedStyle(el).color);
  if (hex) document.getElementById('ep-color').value = hex;
  const ta = getComputedStyle(el).textAlign;
  document.querySelectorAll('[data-a]').forEach(b =>
    b.classList.toggle('on', b.dataset.a === ta || (ta === 'start' && b.dataset.a === 'left'))
  );
}

function epClear() {
  if (epSel) epSel.classList.remove('ep-sel');
  epSel = null;
  document.getElementById('ep-hint').style.display = 'block';
  document.getElementById('ep-ctrl').style.display = 'none';
}

function epAutosize() {
  const ta = document.getElementById('ep-text');
  ta.style.height = 'auto';
  ta.style.height = ta.scrollHeight + 'px';
}

function epAccent(v) {
  document.documentElement.style.setProperty('--accent', v);
  const r = parseInt(v.slice(1,3),16), g = parseInt(v.slice(3,5),16), b = parseInt(v.slice(5,7),16);
  document.documentElement.style.setProperty('--accent-light', `rgba(${r},${g},${b},0.12)`);
}

function epText(v)  { if (epSel) epSel.textContent = v; epAutosize(); }
function epSize(v)  { if (epSel && v) epSel.style.fontSize = v + 'px'; }
function epColor(v) { if (epSel) epSel.style.color = v; }

function epBold() {
  if (!epSel) return;
  const bold = parseInt(getComputedStyle(epSel).fontWeight) >= 600;
  epSel.style.fontWeight = bold ? '400' : '700';
  document.getElementById('ep-bold').classList.toggle('on', !bold);
}

function epAlign(v) {
  if (!epSel) return;
  epSel.style.textAlign = v;
  document.querySelectorAll('[data-a]').forEach(b => b.classList.toggle('on', b.dataset.a === v));
}

function epDownload() {
  // ダウンロード時はOFF状態でシリアライズして保存
  const wasOn = epMode;
  if (wasOn) {
    document.body.classList.remove('edit-mode');
    document.getElementById('ep-panel').classList.remove('open');
    document.getElementById('ep-toggle').textContent = '編集モード OFF';
    document.getElementById('ep-toggle').classList.remove('on');
    if (epSel) epSel.classList.remove('ep-sel');
  }
  const html = '<!DOCTYPE html>\n' + document.documentElement.outerHTML;
  if (wasOn) {
    document.body.classList.add('edit-mode');
    document.getElementById('ep-panel').classList.add('open');
    document.getElementById('ep-toggle').textContent = '編集モード ON';
    document.getElementById('ep-toggle').classList.add('on');
    if (epSel) epSel.classList.add('ep-sel');
  }
  const name = (document.title || 'slide').replace(/[^\w\u3040-\u9fff]/g, '-') + '.html';
  const a = Object.assign(document.createElement('a'), {
    href: URL.createObjectURL(new Blob([html], {type:'text/html;charset=utf-8'})),
    download: name
  });
  document.body.appendChild(a); a.click();
  document.body.removeChild(a); URL.revokeObjectURL(a.href);
}

function epColorToHex(css) {
  const s = (css || '').trim();
  if (/^#[0-9a-f]{6}$/i.test(s)) return s;
  if (/^#[0-9a-f]{3}$/i.test(s)) return '#' + [...s.slice(1)].map(c => c+c).join('');
  const m = s.match(/\d+/g);
  return (m && m.length >= 3)
    ? '#' + m.slice(0,3).map(n => parseInt(n).toString(16).padStart(2,'0')).join('')
    : null;
}
```

#### 注意事項

- `textContent` による上書きで子要素（`<span>` 等）のマークアップが失われる。これは仕様（v1）
- Pattern C（Linear系）の `<span>` キーワード強調は編集後に失われる可能性がある
- `--accent-text` は自動更新しない。アクセント色を淡色に変えた場合、アクセント面上の白テキストが読みにくくなることがある
- `body.edit-mode { padding-right: var(--ep-w) }` はスライドの16:9比率を維持したまま全体を縮小する。`.slide-footer`（ナビ＋トグル）も同様に左にシフトするため、編集モードON時もトグルはスライド右端に揃ったまま

### スタイル定義

#### A. Apple Minimal

```css
:root {
  --bg:             #ffffff;
  --bg-sub:         #f5f4f0;
  --text-primary:   #1d1d1b;
  --text-secondary: #86847c;
  --text-muted:     #aeaca4;
  --border:         #c8c6bc;
  --border-light:   #e0dfd8;
  --accent:         #1d1d1b;
  --accent-text:    #ffffff;
}
```

- ノード・ボックス：`border: 0.5px solid var(--border)` / `border-radius: 6px`
- 強調要素：黒塗り・白テキスト
- フォント：`system-ui, -apple-system, sans-serif`
- アクセントカラーは黒のみ。カラーは一切使わない。

#### B. Google系（Material Design）

コンセプト：白ベースに **Googleの4色を要素ごとに使い分ける**。1色アクセントではなく「複数色で要素・カテゴリを識別させる」のが本質（ここがD Magentaとの決定的な違い）。角丸を大きめにとり、面はやわらかい影で浮かせ（elevation）、チップ／バーはピル形にする。明るくフレンドリーな印象。

```css
:root {
  --bg:             #ffffff;
  --bg-sub:         #f1f3f4;   /* grey 100 */
  --text-primary:   #202124;   /* grey 900 */
  --text-secondary: #5f6368;   /* grey 700 */
  --text-muted:     #9aa0a6;   /* grey 500 */
  --border:         #dadce0;   /* grey 300 */
  --border-light:   #e8eaed;   /* grey 200 */
  /* Googleの4色（要素の識別に使い分ける） */
  --g-blue:   #4285F4;  --g-blue-light:   #e8f0fe;
  --g-red:    #EA4335;  --g-red-light:    #fce8e6;
  --g-yellow: #F9AB00;  --g-yellow-light: #fef7e0;  /* 前景は濃いF9AB00。白地で読めるよう文字は #9a6700 などに落とす */
  --g-green:  #34A853;  --g-green-light:  #e6f4ea;
  /* 単色運用時のフォールバック（既定は青） */
  --accent:         #4285F4;
  --accent-light:   #e8f0fe;
  --accent-text:    #ffffff;
}
```

- フォント：`system-ui, sans-serif`（GoogleはGoogle Sans系。Webフォント不使用方針により `system-ui` で代替）
- カードは角丸大きめ（`border-radius: 16px`、強調カードは20〜22px）。ハードな枠線は外し、やわらかい影で浮かせる：`box-shadow: 0 1px 3px rgba(60,64,67,.16), 0 1px 2px rgba(60,64,67,.1)`
- チップ（eyebrow）・バー・ボタンはピル形：`border-radius: 999px`、タグの `padding` は `10px 20px` を基準とする（視認性確保のため小さくしない）
- アイコン背景は円形のまま。背景に各色の淡色面（`--g-xxx-light`）、アイコンstroke/fillは対応する濃色を使う
- **4色は意味づけして使い分ける**：情報・主役＝青／肯定・成功・最適＝緑／注意・リスク・低下＝赤／注目・ハイライト＝黄。並列カードや手順番号、グラフの系列を色で識別させる
- 1色で塗り続けない。ただし本文の長文には多色を使わず、色はアクセント面・アイコン・図に限定する（可読性のため）

#### C. Linear系

コンセプト：ダークを主役にし、紫(藍)1色のアクセントと隅のオーロラ発光で奥行きを出す。引き算ではなく「静かな作り込み」。テック／スタートアップ寄りの相手に効く。

```css
:root {
  --bg:             #08090A;                /* 純黒ではなく僅かに青みのある黒 */
  --bg-sub:         rgba(255,255,255,0.03);  /* ガラス調カード面 */
  --text-primary:   #F7F8F8;
  --text-secondary: #8A8F98;
  --text-muted:     #62666D;
  --border:         rgba(255,255,255,0.08);  /* ヘアライン */
  --accent:         #7170FF;
  --accent-light:   rgba(113,112,255,0.14);  /* アイコン背景・ハイライト面 */
  --accent-text:    #A5A2FF;                 /* 暗背景でも読める明るめの紫 */
}
```

- フォント：`"Inter", -apple-system, system-ui, sans-serif`（Interを参照するが、Webフォント不使用の方針に従いシステムフォントで代替する）
- 字間をやや詰める：見出しに `letter-spacing: -0.02em`
- 太さは 500〜600 まで。700 は使わない（重く見えるため）
- ノード・カード：`background: var(--bg-sub)` / `border: 1px solid var(--border)` / `border-radius: 8px`。ガラス調の半透明面とする
- アイコン背景：`border-radius: 7px` の角丸スクエア＋ `background: var(--accent-light)`（円形ラップは使わない）
- 強調：本文中のキーワード1語だけ `color: var(--accent-text)` にする（例：転記90分を `<span>10分</span>` へ）
- モチーフは「隅のオーロラ発光」に一点集中する。`radial-gradient(circle, rgba(113,112,255,0.45) 0%, rgba(94,106,210,0.18) 40%, transparent 70%)` を `filter: blur(20px)` でぼかし、タイトル／クロージングの隅に1つだけ置く。多用しない
- 背景の微細ドットグリッド（任意）：`background-image: radial-gradient(rgba(255,255,255,0.045) 1px, transparent 1px)` / `background-size: 22px 22px`
- アクセントは紫1色のみ。他のカラーは加えない。ベタ塗りに紫を多用せず、発光と1語強調に面積を絞る

#### D. Magenta Brand（自社系）

Apple Minimalのパレットをベースに以下を追加：

```css
:root {
  /* Apple Minimalと同じベース変数に加えて： */
  --accent:         #e4007f;
  --accent-light:   #fce8f3;
  --accent-text:    #ffffff;
}
```

- 強調要素（見出し下線・アイコン背景・ボタン）に `--accent` を使用
- 塗りつぶし面積は全体の10〜20%以内に抑える
- テキストに直接マゼンタを使わない（コントラスト確保のため）

### スライドレイアウトパターン

スライドの内容に応じて以下から選ぶ。同一資料内でレイアウトを変化させて単調さを避ける。

| パターン                       | 使いどころ           | 実装の目安                                                                |
| ------------------------------ | -------------------- | ------------------------------------------------------------------------- |
| 左コンテンツ＋右背景色ブロック | タイトルスライド     | 左60%コンテンツ・右40%`var(--bg-sub)`                                     |
| タイトル＋カードグリッド       | 問題列挙・要素紹介   | 3カラムgrid、各カードにアイコン                                           |
| 左テキスト＋右SVG図            | 仕組みの説明・フロー | 左60%・右40%`var(--bg-sub)`。タイトルスライドと比率を揃える               |
| 番号付きリスト                 | 理由・手順・ポイント | 番号に`var(--accent)`を使い視線誘導                                       |
| 2カラム比較（Before/After）    | 変化・対比           | Before側をグレー・After側を`var(--accent-light)`＋`var(--accent)`ボーダー |
| FAQ形式                        | 疑問解消・補足説明   | Q行に`var(--bg-sub)`、A行にインデント                                     |
| ダーク左＋ライト右             | まとめ・クロージング | 左を`var(--text-primary)`背景・白テキスト、右にチェックリスト             |
| 大きな数字＋説明               | 統計・インパクト重視 | 数字60〜72px、`var(--accent)`色                                           |
| テーブル                       | 比較表・仕様一覧     | ヘッダー行に`var(--bg-sub)`                                               |

まとめスライドは **左40% を `var(--text-primary)`（黒）背景・白テキスト**にし、**右60% にチェックリスト**を置くレイアウトを基本とする。タイトルスライド（左60%/右40%）・内容スライド（左60%/右40%）と視覚的に対応し、資料全体のリズムが揃う。締まりが出てプレゼンの終わりを明確に示せる。なお Linear系のみ例外で、ベースが既にダークのため、この手法ではなく大きめのステートメント文＋キーワードの紫強調で締める。

## 出力品質チェック

生成後に以下を自己確認する：

- [ ] すべてのスライドでテキストがボックス内に収まっているか
- [ ] フォントサイズが各レベルの定義に従っているか
- [ ] テキスト量が多くフォントサイズを下げた場合、縮小幅が2〜4px以内に収まっているか
- [ ] 外部リソース（CDN、Webフォント）を参照していないか
- [ ] ナビゲーション（← →キー）が動作するか
- [ ] 非アクティブなスライドが隠れ、1枚ずつ送れるか（全ページが積み重なっていないか）
- [ ] スライド番号が正しく表示されるか
- [ ] スタイル変数がすべての `color` / `background` / `border` に使われているか
- [ ] 装飾的なアクセントライン・派手なヘッダーバーが入っていないか
- [ ] 編集UIが正常に動作するか（編集モードON/OFF・テキスト選択・アクセントカラー変更・ダウンロード）

## 共通ルール

- スキル本体の出力はMarkdown形式
- スライド本体の出力はHTML形式（1ファイルに全スライドを収める）
- 絵文字は使用しない
- 不要な前置き・余談は省く
- 最終確認はブラウザ表示で行う（HTML表示が最終成果物。PowerPoint貼り付けは前提としない）
- ブラウザによるフォント差異は許容範囲。Chromeでの表示を基準とする
- 1スライドあたりの情報量が多い場合は、スライドを分割することを提案する
- ユーザーがスタイルを最初から指定した場合はスタイル選択ステップをスキップする
- SVGの `marker-end`（矢印）を使う場合は `<defs>` 内に定義し、IDの重複に注意する
- Magenta Brandスタイルでは、マゼンタをテキストに直接使わない（白背景上でのコントラスト不足を防ぐため）
- Linear系では、アクセントの紫をベタ塗り面に使いすぎない（発光と1語強調に限定し、暗背景上のコントラストと静けさを保つ）
- Google系では、1色アクセントにせずGoogleの4色を要素ごとに使い分ける（色でカテゴリ・系列・意味を識別させる）。Dの色違いにしないこと。黄は前景に使うと白地で読みにくいため、文字は濃いトーン（例 #9a6700）に落とす
- `tag`（ラベル）コンポーネントは各スライドの性格を示すのに有効。乱用しない
- 修正依頼があった場合、該当スライドのみ変更して全体を再出力する
