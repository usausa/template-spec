# はじめに (新規プロジェクトの立ち上げ)

1. このテンプレートを clone / コピー。ルート名・ソリューション名を自プロジェクトへ変更。
2. `AGENTS.md` の「スタック」節を記入 (.NET 10 / ASP.NET Core / Blazor Server + minimal API 等)。
3. LINT / ビルド設定を確認:
   - `.editorconfig` / `Directory.Build.props` / `Analyzers.ruleset` は流用済み。
   - `Directory.Build.props` が全プロジェクトへ analyzer と `Analyzers.ruleset` を適用する (`src/` `tests/` 配下でも有効)。
4. `docs/architecture/` を自プロジェクト向けに調整 (`api.md` = Web 固有、`common/` = .NET 共通)。
5. ソースを配置 (詳細は `src/README.md` / `tests/README.md`):
   - `src/<App>/` … Web アプリ本体 (Blazor Server + minimal API)
   - `src/<App>.AppHost/`・`src/<App>.ServiceDefaults/` … .NET Aspire
   - `tests/<App>.UnitTests/`・`tests/<App>.IntegrationTests/`
   - ソリューションは `.slnx` (Solution Items に設定ファイルを束ねる)。
6. OpenAPI を有効化 (`AddOpenApi()` / `MapOpenApi()`) し、`/spec-sync` で `docs/reference/api/openapi.json` を生成できるようにする。
7. Claude Code 設定 (`.claude/settings.json` の permissions / hooks) を確認。PowerShell フックに `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。
   - **MCP**: `.mcp.json` に Microsoft Learn と NuGet MCP を同梱 (初回は承認を求める。NuGet MCP は **.NET 10 SDK** の `dnx` が必要)。
   - **skill の追加**(任意)は `運用ガイド.md` を参照。
8. 最初の要求を `docs/requirements/` に書く (`/requirements` で草案生成も可)。

以降の開発の回し方は [workflow.md](workflow.md)。
