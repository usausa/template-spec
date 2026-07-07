---
name: requirements-to-design
description: 要求(REQ)から設計意図(DES)へ落とす手順。ID採番とトレーサビリティ更新を含む。新機能を要求→設計に進めるときに使う。
---

# REQ → DES の進め方

1. 対象 `REQ-*` を読む(無ければ `requirements` サブエージェントで草案)。
2. `designer` サブエージェント(または本手順)で `DES-NNNN-*.md` を作る(採番は既存最大 +1)。
3. DES に書くのは「**何を・なぜ**」= 設計方針・レイヤ配置・トレードオフ。
   引数一覧のような**現状仕様(What/How)は書かない**(コード+テスト＝生成物に委ねる)。
4. `docs/architecture/` の原則と突き合わせ、外す判断は `/adr` で決定として残す。
5. frontmatter を整える: `id: DES-NNNN` / `status` / `related: [REQ-xxxx, ADR-xxxx]`。
6. `REQ` 側の `related` にも `DES` を相互リンク。`/trace` で整合を確認。
