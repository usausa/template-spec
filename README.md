# C# × Claude Code AI駆動開発テンプレート(Web)

C# / .NET(ASP.NET Core Web)× Claude Code の AI駆動開発テンプレート。
Blazor Server + minimal API を想定する。

## 📌 3原則
- **レーンを固定**: C# + Claude 前提。標準は具体的で深い。
- **不変と可変を分離**: engine(`.claude`/`docs`) / 生成物(`docs/reference`) / 環境固有値 を分ける。
- **ドキュメントを腐らせない**: Why=ADR、What/How=生成・テスト、書式=analyzer、更新は hooks / `/done` で変更に埋め込む。

## 📌 スタック / LINT
- .NET 10 / ASP.NET Core(Blazor Server + minimal API)/ Serilog。
- `.editorconfig` + `Directory.Build.props`(`EnforceCodeStyleInBuild` / `WarningsAsErrors` / `AnalysisMode=All`)+ `Analyzers.ruleset`。
- 方針は「**ルールは緩めても警告はゼロ**」。CI ビルド成果物のみを正とする。

## 📁 構成(予定)
- `src/` … アプリ本体・.NET Aspire(AppHost / ServiceDefaults)。詳細は `src/README.md`。
- `tests/` … 単体・結合テスト。詳細は `tests/README.md`。
- ※ 現時点でソースは未配置。`src/`・`tests/` は将来の配置先。

## 🔄 使い方
- 立ち上げ・開発フロー・各ファイルの役割は、ルート直下の **`運用ガイド.md`** を参照。
- 文書の寿命方針は `docs/README.md`、各段階で打つ具体プロンプトは `docs/guides/workflow.md`。
