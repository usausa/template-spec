# docs の地図と「寿命クラス」

このリポジトリのドキュメントは、**寿命(腐りやすさ)で維持方針を分ける**。
何を手で守り、何を生成し、何を追記だけするか —— この表が人にも AI にも効く契約になる。

| ディレクトリ | 種別 | 寿命 | 誰が維持 | 腐り対策(延命方針) |
|---|---|---|---|---|
| `adr/` | **Why(決定理由)** | 不変 | 人(AI 草案) | **追記のみ。過去 ADR は編集しない**。古くなること＝価値 |
| `architecture/` | **原則・規約** | 長 | 人 | 変更時のみ更新。機械強制できる分は `.editorconfig`/analyzer へ委譲 |
| `requirements/` | **意図(要求)** | 中 | 人(AI 草案→承認) | 意図が変わる変更で更新(`/done` が促す) |
| `design/` | **設計意図** | 中 | 人(AI 草案→承認) | 同上。実装詳細は書かず**コードへ委譲** |
| `reference/` | **What/How(現状仕様)** | 短(即腐る) | **生成** | **手書き禁止**。Web API は OpenAPI、振る舞いはテストが正 |
| `traceability/` | 索引 | 派生 | 生成/検査 | `/trace` で ID 整合を検査 |
| コード書式・品質 | What | — | **ツール** | `.editorconfig` + analyzer、**警告0** |

## 使い分けの原則
- **Why は残す**(`adr/`)。**What/How は生成する**(`reference/`)。**書式は機械が守る**(analyzer)。
  → 人が手で守る面積を最小化する。
- `reference/**` は生成物。編集しない(`.claude/settings.json` の deny で禁止)。
- Web API の現状仕様は **OpenAPI**(`Microsoft.AspNetCore.OpenApi`)で生成する(`/spec-sync`)。
  コードのリファレンスサイト(DocFX 等)は作らず、**テスト(実行可能な仕様)**で振る舞いを担保する。

## ID 体系
- `REQ-NNNN`(要求)／`DES-NNNN`(設計意図)／`ADR-NNNN`(決定)。テスト・コードは `traceability/index.md` で対応づける。
- 手書き文書には frontmatter(`id` / `status` / `related`)を付け、`/trace` が機械検査できるようにする。

## 進め方
`guides/workflow.md` を参照(決定→ `/adr`、実装→ build 逆ループ→ `/spec-sync`→ `/review`→ `/done`)。
