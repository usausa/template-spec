# プロジェクト方針 (編集可)

> このプロジェクト固有のコーディング / 設計方針。**チームで編集して育てる**ドキュメント。
> `.editorconfig` や analyzer で機械化できない「意味のルール」を置く。
> AI は `AGENTS.md` 経由でこれに従う。**機械強制ではない**ため、reviewer / Codex / 人のレビューで担保する (soft enforcement)。
> .NET 共通の不変原則は [`common/coding-principles.md`](common/coding-principles.md)、Web API 固有の規約は各アーキ固有ドキュメント。

## コーディング
- 定型 API の静的呼び出しは **BCL 型**で書く: `String.IsNullOrEmpty(x)` (`string.IsNullOrEmpty` としない)。
- 空文字などの**値**はキーワードで書く: `string.Empty` (`String.Empty` としない)。
  - ※「静的呼び出しは BCL 型 / 値参照はキーワード」の使い分けは `.editorconfig` の単一ルールでは表現できないため、ここに明文化しレビューで確認する。

## 追記のしかた
- ここにプロジェクト固有の方針を追記していく。追記したら、reviewer / Codex がその観点でも見るよう [`../review-checklist.md`](../review-checklist.md) にも反映する。
- 機械強制できるものは可能なら `.editorconfig` / `Analyzers.ruleset` へ寄せ、ここには「機械化できない意味ルール」を残す。
