---
description: 設計意図(DES)を designer サブエージェントで草案化する。
argument-hint: [対象の REQ-ID]
allowed-tools: Read, Grep, Glob, Write
---

`designer` サブエージェントに、次の要求に対する設計意図(DES)の草案を依頼する:

対象: $ARGUMENTS

- `docs/design/_template.md` の様式で `DES-NNNN-<title>.md` を作成(採番は既存最大 +1)
- 「**何を・なぜ**」= 方針・レイヤ配置・トレードオフを書く(**現状仕様は書かない**)
- `docs/architecture/` の原則に整合させ、原則を外す判断があれば `/adr` を促す
- frontmatter に id / status: draft / related(対応 REQ・ADR)を付ける。完了後 `DES-ID` を提示
