# Skills導入手順（Claude Code デスクトップアプリ）

## 概要

- Claude Code のデスクトップアプリ（Code タブ）に、GitHub からのスキルを手動でインストールする手順
- `/plugin` コマンドは使用しない
- この手順では **Superpowers** をインストールする

---

## 前提条件

- Claude デスクトップアプリがインストール済みであること
- Claude の有料プラン（Pro / Max / Team / Enterprise）に加入済みであること
- Code タブを一度以上起動していること（`~/.claude` フォルダの自動生成のため）

---

## Windows 向け手順

### 1. リポジトリをダウンロード

1. ブラウザで `https://github.com/obra/superpowers` を開く
2. 緑色の「Code」ボタンをクリック →「Download ZIP」を選択
3. ダウンロードした `superpowers-main.zip` を任意の場所に展開する

### 2. スキルフォルダを作成してコピー

1. エクスプローラーのアドレスバーに以下を入力して Enter を押す
   ```
   %USERPROFILE%\.claude
   ```
2. `skills` フォルダが存在しない場合は新規作成する
3. 展開した ZIP 内の `skills` フォルダの中身（フォルダ群）を
   `%USERPROFILE%\.claude\skills\` にコピーする

### 3. CLAUDE.md を配置

展開した ZIP 内の `CLAUDE.md` を、作業対象プロジェクトのルートフォルダにコピーする。

> Claude Code はセッション開始時にこのファイルを読み込んでスキルを認識する。

### 4. 動作確認

Code タブでプロジェクトフォルダを開き直し、以下を入力する。

```
/using-superpowers
```

以下のメッセージが返れば成功。

```
Superpowers skill loaded. I'll check for and invoke relevant skills before any
response or action — no rationalizing around it.
What would you like to work on?
```

---

## macOS 向け手順

### 1. リポジトリをダウンロード

Windows と同様に `https://github.com/obra/superpowers` から ZIP をダウンロードして展開する。

または、ターミナルで以下を実行しても取得できる。

```bash
cd ~/Downloads
curl -L https://github.com/obra/superpowers/archive/refs/heads/main.zip -o superpowers.zip
unzip superpowers.zip
```

### 2. スキルフォルダを作成してコピー

ターミナルで以下を実行する。

```bash
mkdir -p ~/.claude/skills
cp -r ~/Downloads/superpowers-main/skills/* ~/.claude/skills/
```

Finder を使う場合は以下の手順で操作する。

1. Finder のメニューバーで「移動」→「フォルダへ移動」を選択
2. `~/.claude` と入力して Enter を押す
3. `skills` フォルダが存在しない場合は新規作成する
4. 展開した ZIP 内の `skills` フォルダの**中身**（フォルダ群）をコピーする

> `~/.claude` は隠しフォルダのため、Finder では `⌘ + Shift + .` で隠しファイルを表示する必要がある。

### 3. CLAUDE.md を配置

展開した ZIP 内の `CLAUDE.md` を、作業対象プロジェクトのルートフォルダにコピーする。

```bash
cp ~/Downloads/superpowers-main/CLAUDE.md /path/to/your/project/
```

### 4. 動作確認

Windows と同様に Code タブで `/using-superpowers` を入力して確認する。

---

## 補足

| 項目             | 内容                                                     |
| ---------------- | -------------------------------------------------------- |
| スキルの保存場所 | `~/.claude/skills/`（全プロジェクト共通）                |
| CLAUDE.md の場所 | 各プロジェクトのルートフォルダ（プロジェクトごとに必要） |
| アップデート方法 | GitHub から再度 ZIP をダウンロードして上書きコピー       |
| アンインストール | `~/.claude/skills/` 内の該当フォルダを削除               |

---

## トラブルシューティング

### `.claude` フォルダが見つからない場合
Code タブでフォルダを開いて何か一言送信すると自動生成される。

### スキルが認識されない場合
Code タブでプロジェクトフォルダを開き直す（再起動）してから再度試す。
