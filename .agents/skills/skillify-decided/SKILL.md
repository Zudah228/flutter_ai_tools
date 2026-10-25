---
name: skillify-decided
description: "Watches refactor/skills/TODO.md, improve-usability/skills/TODO.md, improve-test/TODO.md, and struct-common-widget/TODO.md. When a note is added under 決定, moves it to 決定（進行中）, writes one skill, and moves the note to that directory's COMPLETE.md when that skill is finished. On completion, appends the skill name under 決定版の人間レビュー. Use when a bullet appears under ### 1. 決定, when watching any of these TODO.md files, or when asked to skillify a decided note."
---

# skillify-decided

対象は `refactor/skills`、`improve-usability/skills`、`improve-test`、`struct-common-widget`。どれも同じ手順で、ファイルはそれぞれのディレクトリに置く。

`<root>/TODO.md` を監視する。`### 1. 決定` に追加されたら、`### 2. 決定（進行中）` に移して作成する。完成したら `<root>/COMPLETE.md` に移し、その TODO.md の `### 3. 🧑‍💻 決定版の人間レビュー` にスキル名を書く。

`TODO_DRAFT.md` の `### 1. 案` と `### ２. 人間レビュー` は動かさない。ノートの文言は移すとき変えない。`### 3. 🧑‍💻 決定版の人間レビュー` にはスキル名だけを足す。そこにある行はノートとして移さない。

## ノート

`### 1. 決定` のトップレベル `*` 1つと、そのネストが 1 ノート。見出し直下の説明文はノートではない。`*` だけの行は空なので無視する。1 ノートにつきスキルは 1 つ。`<root>/<name>/SKILL.md` に、AGENTS.md の File どおり書く。

`### 2. 決定（進行中）` に置くノートは 1 つ。すでにノートがあるときは、決定から新しいノートを移さず、そのノートを先に完成させる。進行中は対象ごとに 1 つまで。

## 手順

1. `refactor/skills/TODO.md`、`improve-usability/skills/TODO.md`、`improve-test/TODO.md`、`struct-common-widget/TODO.md` を読む。各ファイルを `<root>` として、2 から 5 を独立に行う。
2. 進行中にノートがある:
   - 対応する `<root>/<name>/SKILL.md` が未完成なら、それを書き上げる。
   - 完成していれば、そのノートを進行中から削除し、`<root>/COMPLETE.md` の `### 3. 作成済み` の末尾に同じ文言で足す。ファイルが無ければ `### 3. 作成済み` だけのファイルを作ってから足す。同じタイミングで、その TODO.md の `### 3. 🧑‍💻 決定版の人間レビュー` の末尾に、そのスキルのケバブケース名を `*` 1行で足す。
3. 進行中が空で、決定にノートがある:
   - 先頭の 1 ノートを決定から切り取り、進行中に置く。空の `*` は残さない。作成を始める前にその TODO.md を保存する。
   - そのノートだけから `<root>/<name>/SKILL.md` を作る。ディレクトリ名、`name`、`#` 見出しは同じケバブケース。名前は修正内容から取る。
4. 完成したら、そのノートを進行中から削除し、`### 3. 作成済み` の末尾に同じ文言で足す。`### 3. 🧑‍💻 決定版の人間レビュー` の末尾にスキル名を `*` 1行で足す。決定に次のノートがあれば 3 に戻る。
5. その対象で決定も進行中も空なら、スキルは作らない。

完成とは、`<root>/<name>/SKILL.md` があり、AGENTS.md の File、description、Examples を満たしていること。満たさないあいだは進行中に残し、COMPLETE.md には移さない。完成したときだけ、ディレクトリ名と同じスキル名を `### 3. 🧑‍💻 決定版の人間レビュー` に書く。同じノートを決定と進行中、または進行中と作成済みの両方に置かない。同じスキル名を決定版の人間レビューに重ねて書かない。一方の対象のノートを、ほかの対象の TODO.md や COMPLETE.md に移さない。

## 監視

手順を 1 回実行してから監視する。すでに `AGENT_LOOP_WAKE_skillify` の監視が動いていれば足さない。

リポジトリのルートで、いずれかの TODO.md の更新時刻が変わったときだけ起こす。

```bash
python3 -c '
import json, os, time
paths = ["refactor/skills/TODO.md", "improve-usability/skills/TODO.md", "improve-test/TODO.md", "struct-common-widget/TODO.md"]
last = {path: os.stat(path).st_mtime for path in paths}
while True:
    time.sleep(2)
    changed = []
    for path in paths:
        now = os.stat(path).st_mtime
        if now != last[path]:
            last[path] = now
            changed.append(path)
    if changed:
        prompt = "TODO.md changed: " + ", ".join(changed) + ". Run skillify-decided from step 1."
        print("AGENT_LOOP_WAKE_skillify " + json.dumps({"prompt": prompt}), flush=True)
'
```

`notify_on_output` の pattern は `^AGENT_LOOP_WAKE_skillify`。起きたら最新の `prompt` を読み、手順 1 からやり直す。変わった対象だけを処理する。決定が空のあいだはスキルを作らない。
