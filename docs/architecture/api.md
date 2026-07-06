# アーキテクチャ (minimal API / レイヤ) 【Web 固有】

> このファイルは **ASP.NET Core (Web) アーキテクチャ固有**。.NET 共通の規約は [`common/`](common/) を参照。

## レイヤ (依存は上→下のみ)

```
Program.cs → Application (組み立て/DI/ルート定数)
                 │
     ┌───────────┼───────────┐
     ▼           ▼           ▼
 Endpoints    Components   (ミドルウェア)
 (minimal API)  (Blazor)
     └─────┬─────┘
           ▼
        Services → (DB / 外部通信)      Models=POCO, Domain=純粋ロジック
```

| レイヤ | 責務 |
|---|---|
| Program.cs | 合成起点。builder 構成・Serilog・DI 登録・パイプライン・Map。**薄く保つ** |
| Application | 起動の組み立てを Program から切り出す拡張群、ルート定数 (`ApiRoutes`)、共通ヘルパー |
| Endpoints | minimal API。`MapGroup` + static ハンドラ。`IResult` を返し、ロジックは Service へ委譲 |
| Components | Blazor Server UI (Layout/Pages)。ロジックは書かず Service/Application へ委譲 |
| Services | DB/ファイル/外部通信のプリミティブ。`IOptions<Setting>` で設定注入、DI |
| Models / Domain | POCO / 純粋ロジック |

## minimal API / エンドポイント
- `app.MapGroup("/api/<resource>")` でグルーピングし、拡張メソッド (`MapXxxEndpoints`) で定義。
- ハンドラは static メソッド。依存は引数で受け取り (DI)、戻り値は `IResult`。実処理は Service へ委譲。
- ルートは `Application/ApiRoutes.cs` に定数化。大きいアップロードは `MaxRequestBodySize` / `WithRequestTimeout`。認可は `RequireAuthorization()`。

```csharp
public static void MapFileEndpoints(this WebApplication app)
{
    var group = app.MapGroup(ApiRoutes.Files);          // "/api/files"
    group.MapGet("/{id}", HandleGet);
}

private static IResult HandleGet(string id, FileStorageService storage)
    => storage.Find(id) is { } item ? Results.Ok(item) : Results.NotFound();
```

## Blazor Server
- `AddRazorComponents().AddInteractiveServerComponents()` / `MapRazorComponents<App>().AddInteractiveServerRenderMode()`。
- コンポーネントにロジックを書かず Service / Application へ委譲。

## 異常系の具体 ([common/errors.md](common/errors.md) の実装)
- API は例外でなく `IResult` (`Results.NotFound()` 等)。予期せぬ例外は `AddProblemDetails()` + `UseExceptionHandler()` でグローバル処理 (RFC 7807)。

## ログの具体 ([common/logging.md](common/logging.md) の実装)
- Serilog: `builder.Services.AddSerilog(o => o.ReadFrom.Configuration(builder.Configuration))`。`LoggerMessage` source generator (`Log.cs`)。

## データの具体 ([common/data.md](common/data.md) の実装)
- EF Core 等。接続文字列は `appsettings` + `IOptions` / `GetConnectionString()`。

## OpenAPI (現状仕様の生成)
- `Microsoft.AspNetCore.OpenApi` (.NET 10 内蔵): `AddOpenApi()` / `MapOpenApi()`。
- 仕様書は手で書かず、`/spec-sync` で `docs/reference/api/openapi.json` に生成する。エンドポイントに `.WithName()` / `.Produces<T>()` を付けて意味ある OpenAPI にする。
