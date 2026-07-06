# アーキテクチャ規約の地図

このプロジェクトの設計原則。**AI もこれに従って生成・レビューする**。迷ったらここ。
原則から外す判断は決定なので `/adr` で残す。

## 共通 と 固有 の分類

| 種別 | ファイル | 内容 |
|---|---|---|
| **アーキ固有 (Web)** | [`api.md`](api.md) | レイヤ (Program/Application/Endpoints/Components/Services)、minimal API、Blazor、OpenAPI、異常系/ログ/データの実装 |
| **.NET 共通** | [`common/async.md`](common/async.md) | 非同期処理 |
| **.NET 共通** | [`common/errors.md`](common/errors.md) | 例外・異常系の原則 |
| **.NET 共通** | [`common/logging.md`](common/logging.md) | ログ設計 |
| **.NET 共通** | [`common/data.md`](common/data.md) | DB / データアクセス |
| **.NET 共通** | [`common/conventions.md`](common/conventions.md) | 命名・品質・テスト |

- **`common/`** = .NET 共通。他アーキのテンプレート (MAUI 版など) と**同一内容**。
- **`api.md`** = このアーキ (ASP.NET Core Web) だけの話。MAUI 版では代わりに `mvvm.md` が置かれる。
