---
name: workflow-diagram
description: "業務フローや作業イメージをSVG図として可視化するスキル。「作業イメージ」「フロー図」「業務フロー」「図にして」「スライド」などの依頼、または業務・プロセスを視覚的に整理したい場合に使用する。初回はApple系・Google系の2スタイルを生成してユーザーに選ばせ、スタイル決定後は構成違いの2パターンを生成する。ダーク／ライト両モード対応のSVGを出力する。"
---

# 業務フロー図アシスタント

## 目的

ユーザーの入力から業務フローや作業イメージを読み取り、SVG図として可視化する。フェーズ数・ノード数・レイアウト方向は内容に応じて柔軟に決定する。

## 全体フロー

```
[初回リクエスト]
  → Apple系・Google系を1枚ずつ生成（計2ファイル）
  → ユーザーがスタイルを選択
  → 選択スタイルで構成違い2パターンを生成（計2ファイル）
  → ユーザーが最終パターンを選択・調整
```

## Phase 1: 初回 — スタイル選択

### 内容の読み取りとレイアウト判断

ユーザーの入力から以下を読み取る：

| 項目 | 判断基準 |
|---|---|
| ノード（タスク）名 | 明示されていればそのまま使用。なければ内容から推定して使用 |
| フェーズ・グループ | 「段階」「ステップ」「フェーズ」が示唆されれば反映 |
| 強調ノード | 「最終成果」「完了」「重要」に該当するノードを強調 |
| レイアウト方向 | 下記の基準で判断 |

レイアウト方向の判断基準：

- ノード数 1〜4・順序が単純 → 横並び
- ノード数 5以上・縦の流れが強い → 縦並び
- フェーズが複数・各フェーズ内にノードが複数 → グループ横並び＋グループ内縦並び
- ユーザーが方向を指定した場合はそれを優先

### Apple系・Google系を1枚ずつ生成

同じ内容・同じレイアウト構成で、スタイルだけ異なる2ファイルを出力する：

- `diagram_style_apple.svg` → Apple系デザイン（下記「Apple系デザイン原則」に従う）
- `diagram_style_google.svg` → Google系デザイン（下記「Google系デザイン原則」に従う）

`present_files` で両ファイルを提示し、次のように確認する：
> 「Apple系（モノクロ・ミニマル）とGoogle系（カラーバッジ・カード）、どちらの方向性が合っていますか？」

## Phase 2: スタイル決定後 — 構成違い2パターン

ユーザーがスタイルを選んだら、選択スタイルで構成（レイアウト・強調・情報量）が異なる2パターンを生成する。

パターンの差異の軸（どれか2軸以上を変える）：

| 差異の軸 | パターンA例 | パターンB例 |
|---|---|---|
| レイアウト方向 | 横並び | 縦並び |
| 強調ノード | 最終ノードのみ強調 | 全ノード均一 |
| 情報量 | キャプション・ラベルあり | ノード名のみシンプル |
| グループ化 | フェーズを囲み線でグループ化 | 囲みなし・矢印のみ |

ファイル名：
- `diagram_A.svg`
- `diagram_B.svg`

`present_files` で両ファイルを提示し、次のように確認する：
> 「AとBどちらが近いですか？調整したい点があればお知らせください。」

## Apple系デザイン原則

コンセプト：余白・細線・モノクロで構成。要素を削ぎ落とし、図自体の存在を消す。

カラーパレット（CSS変数）：

```xml
<style>
  :root {
    --bg:           #ffffff;
    --bg-sub:       #f5f4f0;
    --text-primary: #1d1d1b;
    --text-secondary:#86847c;
    --text-muted:   #aeaca4;
    --border:       #c8c6bc;
    --border-light: #e0dfd8;
    --em-fill:      #1d1d1b;
    --em-text:      #ffffff;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg:           #1c1c1e;
      --bg-sub:       #2c2c2e;
      --text-primary: #f5f5f7;
      --text-secondary:#98989d;
      --text-muted:   #6e6e73;
      --border:       #48484a;
      --border-light: #38383a;
      --em-fill:      #f5f5f7;
      --em-text:      #1c1c1e;
    }
  }
</style>
```

ノード：
- 通常：`fill="var(--bg)"` / `stroke="var(--border)"` / `stroke-width="0.5"` / `rx="6"`
- 強調：`fill="var(--em-fill)"` / `fill="var(--em-text)"`（黒塗りつぶし・白テキスト）
- フォント：`system-ui, -apple-system, sans-serif` / `font-size="12"`

矢印：
- `stroke="var(--border)"` / `stroke-width="0.8"`
- 分岐・戻り：`stroke-dasharray="4 3"`

フェーズ囲み線：
- `fill="none"` / `stroke="var(--border-light)"` / `stroke-width="0.5"` / `rx="10"`

ラベル・キャプション：
- `font-size="11"` / `fill="var(--text-muted)"`

禁止：アクセントカラーの使用不可。グレースケールのみ。

## Google系デザイン原則

コンセプト：Googleの4色でフェーズ・状態を色分け。カードとバッジで情報密度を高める。

カラーパレット（CSS変数）：

```xml
<style>
  :root {
    --bg:         #f8f9fa;
    --panel-bg:   #ffffff;
    --sub-bg:     #f1f3f4;
    --text-primary:#202124;
    --text-secondary:#5f6368;
    --text-muted: #9aa0a6;
    --border:     #dadce0;
    --shadow:     drop-shadow(0 1px 3px rgba(0,0,0,0.12));
    --red:        #EA4335;
    --green:      #34A853;
    --blue:       #1A73E8;
    --yellow:     #FBBC04;
    --red-light:  #fce8e6;
    --green-light:#e6f4ea;
    --blue-light: #e8f0fe;
    --yellow-light:#fef9e3;
    --badge-text: #ffffff;
    --arrow:      #9aa0a6;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg:         #202124;
      --panel-bg:   #292a2d;
      --sub-bg:     #35363a;
      --text-primary:#e8eaed;
      --text-secondary:#9aa0a6;
      --text-muted: #5f6368;
      --border:     #3c4043;
      --shadow:     drop-shadow(0 1px 3px rgba(0,0,0,0.4));
      --red:        #f28b82;
      --green:      #81c995;
      --blue:       #8ab4f8;
      --yellow:     #fdd663;
      --red-light:  #3c2929;
      --green-light:#1e3a2a;
      --blue-light: #1a2a4a;
      --yellow-light:#3a3010;
      --badge-text: #202124;
      --arrow:      #5f6368;
    }
  }
</style>
```

カラー割り当て方針：
- 警告・問題・未対応 → `--red`
- 完了・整備済・正常 → `--green`
- 自動化・処理中・強調 → `--blue`
- 注意・保留・確認中 → `--yellow`

ノード：
- 通常：`fill="var(--panel-bg)"` / `stroke="var(--border)"` / `rx="8"`
- 状態付き：`fill="var(--[color]-light)"` / `stroke="var(--[color])"`
- 強調（塗りつぶし）：`fill="var(--blue)"` / テキスト `fill="var(--badge-text)"`
- フォント：`Google Sans, Roboto, system-ui, sans-serif` / `font-size="12"`

バッジ（フェーズヘッダー）：
- `rx="11"` 角丸 / `fill` にフェーズカラー / テキスト `fill="var(--badge-text)"`

矢印：
- `stroke="var(--arrow)"` / `stroke-width="1.5"`
- 分岐・戻り：`stroke-dasharray="4 3"`

禁止：Googleの4色（赤・緑・青・黄）を超えるカラーの追加不可。

## 出力品質チェック（両スタイル共通）

- [ ] CSS変数がすべての `fill` / `stroke` / テキスト `fill` に使われているか
- [ ] `@media (prefers-color-scheme: dark)` が正しく記述されているか
- [ ] SVG背景に `<rect width="100%" height="100%" fill="var(--bg)"/>` が設定されているか
- [ ] フォント指定が各スタイルの原則に従っているか
- [ ] SVGサイズ（width/height）がレイアウトに合わせて調整されているか
- [ ] Phase 1では2スタイル、Phase 2では2パターン、それぞれ出力されているか

## 注意事項

- PowerPoint / Word に貼り付けた場合、`prefers-color-scheme` は無視されライトモード固定になる。その旨をユーザーに伝える。
- ノード数が増えた場合はSVGサイズを拡大する（1ノード追加ごとに縦方向約50px目安）。
- ユーザーがスタイルを最初から指定した場合（「Apple風で」など）はPhase 1をスキップしてPhase 2から開始する。

## 共通ルール

- 出力はMarkdown形式（SVGファイル本体を除く）
- 絵文字は使用しない
- 不要な前置き・余談は省く
- SVGファイルは `present_files` で提示する
````