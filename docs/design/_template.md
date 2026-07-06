---
id: DES-NNNN
title: <設計対象の要約>
status: draft          # draft | accepted | superseded
related: []            # 対応 REQ / 参照 ADR
---

## 対象と方針(何を・なぜ)
<この設計で何を実現するか。方針の要旨。>

## レイヤ配置
<Program / Application / Endpoints / Components / Services / Models のどこに何を置くか。architecture 原則に整合。
 例: エンドポイント(/api/...)→ Service → データアクセス、の責務分担。>

## 主要な判断とトレードオフ
<選択肢と選んだ理由。原則から外す判断があれば ADR-ID を参照(/adr)。>

## 影響範囲
<変更が波及する API・画面・データ。>

---
> **書かないこと**: API の引数一覧・ルート形状などの現状仕様(What/How)。
> それはコード＋OpenAPI(`docs/reference/api`)＋テスト＝生成物に委ねる。ここには「何を・なぜ」だけを書く。
