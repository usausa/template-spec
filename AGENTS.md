# プロジェクト規約(Claude / Codex 共通)

> このファイルが唯一の「正」。`CLAUDE.md` はこれを import するだけ。**短く保ち**、詳細は `docs/` と `.claude/skills/` に置く。

## スタック
- 言語: C# / .NET 10(`LangVersion=preview`, `Nullable=enable`, `ImplicitUsings=enable`)
- 種別: ASP.NET Core Web(**Blazor Server + minimal API**)。ログは Serilog
- ソースは `src/`、テストは `tests/`(.NET Aspire の AppHost / ServiceDefaults も `src/` 配下)
- 環境固有値(接続先・キー)はリポジトリに実値を置かず、`appsettings` + ユーザーシークレット / 環境変数で扱う

## コーディング
- 書式は `.editorconfig` に従う(唯一の正。ここには書式を書かない)
- メンバ変数に `_` プレフィックスを付けない
- **ビルド警告ゼロが完了条件**。抑制が必要なときは適用前に確認する(局所は `#pragma warning disable/restore`、恒久は `GlobalSuppressions.cs`)
- アーキテクチャは `docs/architecture/` に従う(Program / Application / Endpoints / Components / Services / Models の責務、
  API の異常系は例外でなく `IResult` ＋ ProblemDetails、`throw;` でスタックトレース保持、async 規約)

## ドキュメント規律(動態・最重要)
- **決定(Why)** を伴う変更 → `docs/adr/` に**追記**(過去の ADR は編集しない)。`/adr` を使う
- **現状仕様(What/How)** → 手で書かない。
  - **Web API は OpenAPI を生成**(`/spec-sync` → `docs/reference/api/openapi.json`)
  - 振る舞いは**テスト(実行可能な仕様)**が正
- **意図(要求/設計)** が変わる → 同じ変更(PR)内で `docs/requirements/`・`docs/design/` を更新
- コードを変えたら必ず自問：「この変更で影響を受ける docs は？」→ `/done` が検査する
- `docs/reference/**` は生成物。**手編集しない**

## 完了条件(DoD)
- `dotnet build` 警告ゼロ ／ 影響 docs 更新済み ／ 決定は ADR に記録 ／ `/trace` 整合
- Git 操作(commit / push)は**人間が実行**する(AI はコマンドを提示するのみ)

## 記述
- ドキュメント・コメントは日本語
