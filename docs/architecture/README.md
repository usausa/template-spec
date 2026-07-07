# アーキテクチャ規約の地図

このプロジェクトの設計原則。**AI もこれに従って生成・レビューする**。迷ったらここ。
原則から外す判断は決定なので `/adr` で残す。

## 📁 分類

| 種別 | ファイル | 内容 |
|---|---|---|
| **アーキ固有 (Web)** | [`api.md`](api.md) | レイヤ (Program/Application/Endpoints/Services)、minimal API、OpenAPI、API 命名・サーバ側セキュリティ |
| **アーキ固有 (Web)** | [`blazor.md`](blazor.md) | Blazor Server コンポーネント、UI/UX、UI 側セキュリティ |
| **プロジェクト方針 (編集可)** | [`conventions.md`](conventions.md) | analyzer で機械化できない、このPJ固有のコーディング / 設計方針 |
| **.NET 共通** | [`common/coding-principles.md`](common/coding-principles.md) | 命名・品質・テストの原則 |
| **.NET 共通** | [`common/async.md`](common/async.md) | 非同期処理 |
| **.NET 共通** | [`common/errors.md`](common/errors.md) | 例外・異常系の原則 |
| **.NET 共通** | [`common/logging.md`](common/logging.md) | ログ設計 |
| **.NET 共通** | [`common/data.md`](common/data.md) | DB / データアクセス |
| **.NET 共通** | [`common/security.md`](common/security.md) | セキュリティ標準 |

- **`common/`** = .NET 共通。他アーキのテンプレート (MAUI 版など) と**同一内容**。
- **`conventions.md`** = このプロジェクトで編集して育てる方針 (機械強制でなくレビューで担保)。
- **`api.md` / `blazor.md`** = このアーキ (ASP.NET Core Web) だけの話。MAUI 版では代わりに `maui.md`。
