---
name: csharp-layered-feature
description: このプロジェクトの層構成(Program/Application/Endpoints/Components/Services/Models)に沿って ASP.NET Core の機能を追加する手順。API エンドポイント追加、Blazor ページ追加、サービス追加のときに使う。
---

# ASP.NET Core レイヤ機能追加の手順

> 詳細な禁則・理由は `docs/architecture/*.md` を参照。ここは順序と要点のみ。

1. **設計確認**: 対象の `DES-*` を確認(無ければ `requirements`→`designer` で作る)。
2. **Models**: リクエスト/レスポンス/エンティティの POCO を定義。
3. **Services**: DB/ファイル/外部通信のプリミティブを実装。`IOptions<Setting>` で設定注入、DI 登録。上位を参照しない。
4. **Endpoints**(API の場合): `app.MapGroup("/api/...")` + static ハンドラ。`IResult` を返し、ロジックは Service へ委譲。
   ルートは `Application/ApiRoutes.cs` に定数化。詳細は `docs/architecture/api.md`。
5. **Components**(画面の場合): Blazor コンポーネント。ロジックは書かず Service/Application へ委譲。
6. **Program/Application**: DI 登録・`MapXxxEndpoints`・ミドルウェアの結線は `Application/` の拡張へ。`Program.cs` は薄く。
7. **テスト**(`tests/`): 受け入れ条件を xUnit で。テスト名を仕様として書く。結合は `WebApplicationFactory`。
8. **仕上げ**: `dotnet build`(警告0)→ API を変えたら `/spec-sync`(OpenAPI 再生成)→ `/review` → `/done`。

## アンチパターン(`docs/architecture` 準拠)
- Endpoint/Component にロジックを書く(→ Service/Domain へ委譲)
- API の異常系を例外で通知(→ `IResult` ＋ ProblemDetails)
- `Task.Wait()` / `Task.Result` / 不要な `Task.Run()`(→ `await`)
- SQL で表示用加工(→ 上位で加工)
