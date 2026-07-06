---
description: 要求(REQ)を requirements サブエージェントで草案化する。
argument-hint: [機能の一言説明]
allowed-tools: Read, Grep, Glob, Write
---

`requirements` サブエージェントに、次の機能の要求(REQ)草案を依頼する:

$ARGUMENTS

- `docs/requirements/_template.md` の様式で `REQ-NNNN-<title>.md` を作成(採番は既存最大 +1)
- 目的/背景・利用者・受け入れ条件(Given/When/Then)・非機能要件を埋める
- **曖昧点は勝手に決めず、質問として列挙**する
- 完了後、作成した `REQ-ID` と未決事項を提示し、人の承認を求める
