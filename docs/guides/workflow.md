# 開発ワークフロー (人が読む運用手順書)

> この文書は**人向けの運用手順書**。`CLAUDE.md` / `AGENTS.md` / `docs/architecture/*` のように
> AI が常時ロードする文書ではない。**機能を 1 つ作る/直すたびに開き**、各段階で下の「打つもの」を実行する。
> AI 側の振る舞いは、`/`コマンド・skill・hook が呼ばれたときにそれぞれのファイルが駆動する。

## 読む文書の使い分け
- **AI が常時従う**: `CLAUDE.md`→`AGENTS.md` (規約) / `docs/architecture/*` (原則) / `docs/README.md` (寿命表)
- **AI は呼ばれた時だけ動く**: `.claude/commands`・`skills`・`agents`・`hooks`
- **人が読む (この文書・`README`・`getting-started`)**: どの順で何を打つか

---

## 全体像 (ループ)

```
決定が要る？ ──yes──▶ /adr (Why を残す・過去 ADR は編集しない)
    │no
    ▼
要求/設計の意図が要る？ ──▶ /requirements → /design で REQ/DES 草案 → 人が承認
    │
    ▼
実装 (skill: csharp-layered-feature / Endpoint・Service・Component の責務に沿って)
    │
    ▼  PostToolUse フック: dotnet format 検証 → 差分を Claude に返す → 直す (逆ループ)
    ▼
/spec-sync (Web API を変えたら OpenAPI 再生成)
    │
    ▼
意図(REQ/DES)が変わったなら同じ変更内で更新
    │
    ▼
/review (reviewer)  →  /done (DoDゲート)  →  人間が git commit
```

---

## 各段階: 具体的に何を打つか

> 例文はコピペしてそのまま使える。`<...>` は置き換える。

### 1. 要求を作る (REQ)
- **打つもの**: `/requirements <機能の一言説明>`
- **例**: `/requirements ファイルのタグ付けと、タグでの絞り込み検索`
- **触るファイル**: `docs/requirements/REQ-000X-*.md` (新規草案) / 雛形 `_template.md`
- **ポイント**: 出てきた「未決事項」に答えてから承認。

### 2. 設計意図を作る (DES)
- **打つもの**: `/design <REQ-ID>`
- **例**: `/design REQ-0002`
- **触るファイル**: `docs/design/DES-000X-*.md` / 参照 `docs/architecture/*`
- **ポイント**: 「何を・なぜ」だけ。API の引数一覧などの現状仕様は書かせない。

### 3. 決定を残す (ADR) ※決定・トレードオフがあれば
- **打つもの**: `/adr <決定の一言>`
- **例**: `/adr タグ検索は DB の全文検索でなく LIKE + インデックスで実装する`
- **触るファイル**: `docs/adr/000X-*.md` (追記) / `docs/adr/index.md`

### 4. 実装する
- **打つもの**: 自然文でよい (`csharp-layered-feature` スキルが自動ロード)
- **例**:
  ```
  DES-0002 に沿ってタグ機能を実装して。
  csharp-layered-feature の手順で Endpoint (minimal API) → Service → Model の責務を守り、
  異常系は IResult + ProblemDetails、ビルド警告はゼロにすること。
  ```
- **自動**: 編集のたびに `hooks/dotnet-verify.ps1` が `dotnet format` 検証→差分を返す (逆ループ)
- **触るファイル**: `src/`(コード) / 参照 `docs/architecture/api.md`・`docs/design/DES-*`

### 5. 現状仕様を同期する (reference) ※Web API・型・DB を変えたら
- **打つもの**: `/spec-sync`
- **触るファイル**: `docs/reference/api/openapi.json` (生成) / `docs/reference/db/` (任意)
- **ポイント**: `docs/reference/**` は手編集禁止 (settings.json の deny)。

### 6. 意図の更新 (REQ/DES) ※意図が変わったら
- **打つもの**: 自然文
- **例**: `今回の実装で REQ-0002 / DES-0002 の意図と変わった点があれば該当文書を更新して。無ければ「変更なし」と報告して。`

### 7. レビュー
- **打つもの**: `/review`
- **ポイント**: Critical が残る間は先へ進まない。指摘ごとの次アクション (`/adr`・`/spec-sync` 等) に従う。

### 8. 完了ゲート
- **打つもの**: `/done`
- **やること**: `dotnet build` (警告0) / `docs` 更新 / ADR 有無 / `/trace` 整合 を一括判定。最後に AI が git コマンドを提示。

### 9. コミット (人が実行)
- AI が提示した git コマンドを人が実行。commit / push は必ず人。

---

## コマンド早見表

| コマンド | いつ | 主体 |
|---|---|---|
| `/requirements <一言>` | 要求 REQ 草案 | 人 |
| `/design <REQ-ID>` | 設計意図 DES 草案 | 人 |
| `/adr <決定>` | 決定・トレードオフを残す | 人 |
| (自然文で実装依頼) | 実装 (skill 自動ロード) | 人→AI |
| `/spec-sync` | Web API/型/DB を変えた | 人 |
| `/review` | 変更のレビュー | 人 |
| `/trace` | ID 整合・決定漏れ検査 | 人 |
| `/done` | 仕上げの DoD ゲート | 人 |

---

## 途中参加・別セッションからの再開

- 新しいチャットでも `CLAUDE.md` (→ `AGENTS.md`・`docs/architecture/*`) は自動で効くので、規約・原則は引き継がれる。
- まず現在地を掴む: `docs/README.md` (寿命表) と `docs/traceability/index.md` (対応表) を見る。
- 続きから: `/trace` で未整合・決定漏れを洗い出し、該当段階のコマンドを打つ。
- このドキュメントフレームワークは**会話履歴を前提とした短い引き継ぎに依存しない**設計思想を採る。工程の状態は成果物ファイル (REQ/DES/ADR/trace) に外部化されるため、履歴が無い新セッションでも現在地をファイルから復元できる。
