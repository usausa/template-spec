---
description: 変更完了前の Definition of Done ゲート。未達なら未達項目を列挙して停止する。
allowed-tools: Bash(dotnet:*), Read, Grep, Glob
---

以下を順に検査し、結果を表 (`[項目 | 状態 | 対応]`) で示す。**1 つでも未達なら「未完了」**と結論する。

1. **ビルド警告ゼロ**か  →  !`dotnet build`
2. 公開 API / 型に変更があるか。あれば `docs/reference` が再生成済みか (要なら `/spec-sync`)
3. この変更は**意図** (`REQ` / `DES`) を変えるか。変えるなら該当文書が更新されているか
4. **設計上の決定**をしたか。したなら該当 ADR が追記されているか (要なら `/adr`)
5. トレーサビリティ整合 (`/trace`)
6. **レビュー観点** (`docs/review-checklist.md`) を満たすか (`/review`、必要なら Codex で `/cross-review`)

最後に:
- 「今回の変更で影響を受ける docs」チェックリスト
- 人間が実行する git コマンド案 (commit / push は人間が実行。AI は提示のみ)
