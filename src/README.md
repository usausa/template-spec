# src (アプリケーション・Aspire の配置先)

> 現時点でソースは未配置。ここは将来の配置先。実装は `/requirements` → `/design` → 実装 (skill) の流れで作る。

## 想定構成

```
src/
  <App>/                    Web アプリ本体 (Blazor Server + minimal API)  [Microsoft.NET.Sdk.Web]
  <App>.AppHost/            .NET Aspire AppHost (オーケストレーション)       [Aspire.AppHost.Sdk]
  <App>.ServiceDefaults/    Aspire ServiceDefaults (OpenTelemetry / health / resilience / service discovery)
```

## メモ
- レイヤ構成は `docs/architecture/api.md`。`Program.cs` は薄く、組み立ては `Application/` へ。
- `<App>.AppHost` は アプリ本体と依存リソース (DB / Redis 等) をオーケストレーションする。
- `<App>.ServiceDefaults` は アプリ本体から参照し、テレメトリ / ヘルスチェック / 回復性を付与する。
- 各 `csproj` は root の `Directory.Build.props` から analyzer・`Analyzers.ruleset`・Nullable 等を継承する (個別設定は不要)。
- ソリューションは root の `.slnx`。
