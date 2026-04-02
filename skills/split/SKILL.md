---
name: split
description: "cmux ペイン分岐。現在の会話を継続する Claude Code セッションを同一サーフェイス内の新しい cmux ペインに起動する。/split で呼び出す。"
---

# Branch（cmux ペイン分岐）

現在の会話を継続する Claude Code セッションを、同一サーフェイス内の新しい cmux ペインに起動する。
元のセッション（このペイン）はそのまま別の作業に使える。

## 手順

以下の Bash コマンドを **順番に実行** すること。途中で失敗したらエラーを報告して終了する。

### Step 1: cmux 環境チェック

```bash
cmux identify
```

- 成功したら JSON 出力の `caller.workspace_ref` の値（例: `workspace:15`）を控える
- 失敗（コマンドが見つからない等）したら「cmux 環境外では /split は使えません」と報告して終了

### Step 2: ペイン分割

```bash
cmux new-split right --workspace <Step1で取得した workspace_ref>
```

- 出力例: `OK surface:42 workspace:15`
- `surface:XX` の部分を控える

### Step 3: Claude Code 起動

```bash
cmux send --surface <Step2で取得した surface_ref> "claude -c\n"
```

### Step 4: 報告

「右ペインに元セッションを起動しました。このペインでは新しい作業を開始できます。」と報告する。
