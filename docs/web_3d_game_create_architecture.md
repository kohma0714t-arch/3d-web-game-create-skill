# web-3d-game-create マルチサブエージェント・詳細アーキテクチャ設計書 (確定版)

## 1. 概要 (Overview)
`web-3d-game-create` は、**Three.js**、**3D物理エンジン (Cannon-es / Rapier3D)**、**Vite + TypeScript** を主軸とし、ブラウザ上で動作する本格的な3Dゲーム（3Dアクション、FPS/TPS、レーシング、迷路脱出、3Dパズル等）を自律的にゼロから高品質に構築するための**親オーケストレータースキル**です。

2D版 `web-game-create` の優れた「スタジオ型・職能分担メタファー」と「コンテキスト伝達＋自動修復ループ」を3Dグラフィックス開発に特化・最適化し、親スキルと **5つの3D専門サブエージェント** が連携して完全自己完結する制作パイプラインを提供します。

---

## 2. スタジオ型・職能分担メタファー (Team Roles for 3D)

| スキル名 | スタジオの役職 | 担当領域 | 主なミッションと成果物 |
| :--- | :--- | :--- | :--- |
| **`web-3d-game-create`** | **統括3Dゲームプロデューサー** | 企画・GDD策定・進行管理 | ユーザーの想いをヒアリングして企画書(`docs/gdd.md`)を策定し、5つの3D専門サブエージェントを順次指揮・統率する |
| **`game-3d-asset-specialist`** | **3Dアートディレクター / モデラー** | 形状・テクスチャ・マテリアル | `generate_image` を用いたUVテクスチャやスカイボックスの生成、Three.jsプロシージャルメッシュ（複合プリミティブ合成）、PBRマテリアル定義(`src/assets/asset-manifest.ts`)を出力 |
| **`game-3d-audio-specialist`** | **3D音響監督 / サウンドデザイナー** | 3D立体音響 ＆ プロシージャルBGM | Web Audio API の `PannerNode` や Three.js `PositionalAudio` / `AudioListener` を駆使し、3D空間上の位置依存サウンド・ドップラー効果・プロシージャルBGMマネージャー(`src/audio/sound-manager.ts`)を構築 |
| **`game-3d-logic-specialist`** | **リード3Dプログラマー / 物理エンジニア** | 3D物理・キャラクター操作・コアループ | 3D物理ワールド（Cannon-es/Rapier）、FPS/TPSキャラクター操作、ジャンプ/重力、レイキャスト衝突判定、敵AI、ゲームバランス定数(`src/config/constants.ts`)を型安全に実装 |
| **`game-3d-scene-specialist`** | **テクニカルディレクター / 演出照明家** | 3Dシーン統合・照明・Juice演出・HUD | Three.js レンダラー、シーン、ライティング（Directional/Ambient/Shadow）、カメラ制御、ポストプロセス（Bloom/画面揺れ/速度線）、2DオーバーレイHUD、`src/main.ts` を統合実装 |
| **`game-3d-qa-specialist`** | **3D QAエンジニア / 最適化デバッガー** | ビルド検証・描画負荷検証・自動修復 | `npm run build`、`tsc --noEmit`、ドローコールやテクスチャ破棄チェックを実施し、不合格時は原因カテゴリ別に担当エージェントへ自動修復を指示（最大3回ループ） |

---

## 3. 確定技術スタック (Technology Stack)

- **ビルドツール**: Vite + Vanilla TypeScript (`npm create vite@latest ./ -- --template vanilla-ts`)
- **3Dレンダリングエンジン**: **Three.js** (`three`, `@types/three`)
- **3D物理演算**: **Cannon-es** (`cannon-es`) または **Rapier3D** (`@dimforge/rapier3d-compat`)
- **3Dアセット戦略**:
  - 外部の巨大な3Dモデル（glTF等）のダウンロードに依存せず、**AI画像生成(`generate_image`)によるテクスチャマップ** ＋ **Three.jsのジオメトリを組み合わせたプロシージャル合成（ローポリ・ボクセル・スタイライズド造形）** を採用。
  - これにより、通信エラーやCORS問題なしで100%確実に動作するアセット調達を実現。
- **3D音響**: Web Audio API + Three.js `AudioListener` / `PositionalAudio`（外部音声ファイル不要のプログラマブル合成）
- **HUD / UI**: 3Dキャンバスの上に重ねる CSS / HTML5 2Dオーバーレイ（視認性の高いサイバーパンク/モダンUI）

---

## 4. 全体開発パイプライン (Pipeline & Sequence)

```mermaid
graph TD
    User([ユーザー]) <-->|1. 3Dゲーム構想・ヒアリング| Lead[web-3d-game-create<br/>親プロデューサー]
    Lead -->|2. GDD策定 & プロジェクト初期化| GDD[docs/gdd.md<br/>3Dゲームデザイン仕様書]

    subgraph "フェーズA: 3Dアセット・マテリアル & 3D音響制作"
        GDD -->|ビジュアル・形状要件| Asset[game-3d-asset-specialist]
        GDD -->|3Dサウンド要件| Audio[game-3d-audio-specialist]
        Asset -->|テクスチャ画像群| P1[public/assets/textures/*.png]
        Asset -->|メッシュビルダー & マテリアル| M1[src/assets/asset-manifest.ts]
        Audio -->|3D立体音響マネージャー| M2[src/audio/sound-manager.ts]
    end

    subgraph "フェーズB: 3D物理・操作・エンティティ開発"
        GDD & M1 --> Logic[game-3d-logic-specialist]
        Logic -->|3Dプレイヤー/敵/当たり判定| ENT[src/entities/*]
        Logic -->|物理ワールド/スポナー/ルール| SYS[src/systems/*]
        Logic -->|ゲームパラメータ| CFG[src/config/constants.ts]
    end

    subgraph "フェーズC: 3Dシーン統合・照明・演出・HUD"
        M1 & M2 & ENT & SYS & CFG --> Scene[game-3d-scene-specialist]
        Scene -->|ライト/カメラ/シャドウ/ポストプロセス| SCNS[src/scenes/*]
        Scene -->|3D Juice演出 / 画面揺れ / HUD| JUICE[Game Feel & HUD]
        Scene -->|エントリーポイント| MAIN[src/main.ts]
    end

    subgraph "フェーズD: 3D自律ビルド検証 & 自動修復ループ"
        SCNS & MAIN --> QA[game-3d-qa-specialist]
        QA -->|ビルド & 型検査| BUILD[npm run build & tsc]
        BUILD -->|合格: PASS| Complete([完成・引き継ぎドキュメント])
        BUILD -->|不合格: FAIL (最大3回)| Analysis[エラー原因分類]
        Analysis -->|マテリアル・テクスチャ不整合| Asset
        Analysis -->|3D音響・リスナーエラー| Audio
        Analysis -->|物理演算・型・ベクトル不整合| Logic
        Analysis -->|レンダラー・シーン・シェーダーエラー| Scene
    end
```

---

## 5. コンテキスト伝達フォーマット (`<SUBAGENT_CONTEXT>`)

各サブエージェントをディスパッチする際は、以下の統一コンテキストパックをプロンプト冒頭に埋め込みます：

```markdown
<SUBAGENT_CONTEXT>
- **対象ゲーム名**: {ゲームタイトル}
- **参照仕様書**: docs/gdd.md
- **担当スペシャリスト**: {game-3d-asset-specialist / game-3d-audio-specialist / ...}
- **3Dエンジン構成**: Three.js + Cannon-es (または Rapier3D) + Vite + TypeScript
- **前提条件・参照成果物**:
  - 3Dアセット定義: src/assets/asset-manifest.ts
  - 3Dサウンド定義: src/audio/sound-manager.ts
  - 3D物理・ロジック: src/entities/ & src/systems/
- **実装対象ファイル**: {ファイル一覧}
- **品質規約**: プレースホルダーの禁止 / 3D Juice演出（ライティング・カメラシェイク・パーティクル）の徹底 / TypeScript Strict準拠
</SUBAGENT_CONTEXT>
```

---

## 6. 自律修復プロトコル (Auto-Repair Protocol)
1. `game-3d-qa-specialist` がビルド失敗や型エラー、リソース解放不備を検出。
2. エラーメッセージから原因カテゴリを分類：
   - **ASSET_ERROR**（テクスチャロード不整合、メッシュ生成関数シグネチャ不一致）→ `game-3d-asset-specialist`
   - **AUDIO_ERROR**（AudioListener初期化エラー、Web Audio 3Dノード未定義）→ `game-3d-audio-specialist`
   - **PHYSICS_LOGIC_ERROR**（Cannon-es/Rapier3Dの型不整合、ベクトル演算エラー、未定義プロパティ）→ `game-3d-logic-specialist`
   - **SCENE_RENDER_ERROR**（Three.jsレンダラー設定、シャドウマップ設定、カメラ初期化漏れ）→ `game-3d-scene-specialist`
3. 修正後、直ちに再ビルドを実行（最大3回まで自律リトライ）。
