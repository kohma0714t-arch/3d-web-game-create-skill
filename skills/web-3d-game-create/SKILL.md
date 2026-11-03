---
name: web-3d-game-create
description: Three.js / Cannon-es (または Rapier3D) / Vite / TypeScript を主軸に、企画・3D-GDD策定から5つの専門3Dサブエージェント（game-3d-asset-specialist, game-3d-audio-specialist, game-3d-logic-specialist, game-3d-scene-specialist, game-3d-qa-specialist）を統括・ディスパッチし、自律的に高品質なWeb3Dブラウザゲームを制作する親オーケストレータースキル。ユーザーが「/web-3d-game-create」「/3d-game-create」「Web3Dゲームを作りたい」「3Dブラウザゲームを作りたい」「3Dゲームを作りたい」「Three.jsでゲーム」「三人称アクション」「FPSゲーム」「3Dドライビング」などに言及した場合は必ずこのスキルを使用すること。
---

# Web 3D Game Creator (マルチサブエージェント・3Dゲーム制作親オーケストレーター)

このスキルは、**Three.js**、**3D物理エンジン (Cannon-es / Rapier3D)**、**Vite + TypeScript** を主軸とし、本格的なWeb3Dゲームをゼロから最高品質で構築するための**親オーケストレータースキル**です。

1つのAIにすべてを任せるのではなく、開発の各フェーズをゲーム開発スタジオの専門職に見立てた **5つの専門3Dサブエージェント** に細分化・委譲し、明確なコンテキスト伝達と最大3回のリトライ修復ループによって、確実に遊べるWeb3Dゲームを自律完成させます。

---

## 専門3Dサブエージェント編成と職能

| 専門サブエージェントスキル | 担当フェーズ | 主な職能と成果物 |
| :--- | :--- | :--- |
| **`game-3d-asset-specialist`** | 3D形状・テクスチャ・マテリアル | `generate_image` を活用したテクスチャ/スカイボックス生成、プロシージャルメッシュ（ジオメトリ合成）、PBRマテリアル構築と `src/assets/asset-manifest.ts` の出力 |
| **`game-3d-audio-specialist`** | 3D立体音響 ＆ プロシージャルSE/BGM | Web Audio API / PositionalAudio による音源位置依存サウンド（PannerNode）、ドップラー効果、BGMシンセ（`src/audio/sound-manager.ts`）の実装 |
| **`game-3d-logic-specialist`** | 3D物理・キャラクター操作・コアループ | 3D物理ワールド（Cannon-es/Rapier）、FPS/TPSキャラクター操作、ジャンプ/重力、レイキャスト衝突判定、敵AI、定数定義（`src/entities/*`, `src/systems/*`, `src/config/constants.ts`）の実装 |
| **`game-3d-scene-specialist`** | 3Dシーン統合・照明・Juice演出・HUD | Three.js レンダラー、シーン、ライティング・シャドウ、カメラ制御、ポストプロセス（Bloom/画面揺れ）、2DオーバーレイHUD、`src/main.ts` を統合実装 |
| **`game-3d-qa-specialist`** | 3Dビルド検証・最適化・自動修復 | `npm run build`、`tsc --noEmit`、ドローコール最適化・テクスチャ破棄確認と、原因別自動修復ディスパッチ（最大3回ループ） |

---

## 実行オーケストレーション・ワークフロー (Sequence)

### Phase 1: 要件ヒアリング & 3D-GDD（ゲームデザイン仕様書）策定
- ユーザーに対話形式でゲームジャンル（TPS/FPS/トップダウン等）、カメラ視点、操作感、世界観を確認。
- **`docs/gdd.md`** に以下を明確に記録：
  - コアループ（3D移動/エイム → アクション/攻撃 → 物理フィードバック → 目標達成）
  - カメラ・操作仕様（キーボード WASD + マウス視点移動 / タッチ対応）
  - ライティングと雰囲気（サイバーパンク、SF、トゥーン調、ダンジョンなど）
  - ゲームオーバー・クリア条件、難易度パラメータ

### Phase 2: プロジェクト初期化 (Scaffolding)
- Vite + TypeScript を非対話モードでセットアップし、Three.js および 物理エンジンをインストール：
  ```bash
  npm create vite@latest ./ -- --template vanilla-ts
  npm install three cannon-es
  npm install -D @types/three
  ```

### Phase 3: 3D素材・立体音響専門エージェントのディスパッチ (並行/順次)
1. **`game-3d-asset-specialist` を起動**:
   - GDDに基づき、`generate_image` でプレースホルダーなしのテクスチャやスカイボックスを生成。
   - Three.js プリミティブを組み合わせたプロシージャルメッシュ定義関数を含む `src/assets/asset-manifest.ts` を出力。
2. **`game-3d-audio-specialist` を起動**:
   - GDDに基づき、3D空間上の音源位置（PositionalAudio）や環境音を制御する `src/audio/sound-manager.ts` を出力。

### Phase 4: 3D物理・エンティティ専門エージェントのディスパッチ
3. **`game-3d-logic-specialist` を起動**:
   - アセット定義とGDDを渡し、`PlayerController.ts`（物理連動）、`EnemyEntity.ts`、`PhysicsWorld.ts`、`CollisionSystem.ts` を実装。
   - `src/config/constants.ts` に移動速度、重力、弾速などのゲームバランス定数を整理。

### Phase 5: 3Dシーン・演出・HUD統合専門エージェントのディスパッチ
4. **`game-3d-scene-specialist` を起動**:
   - 全素材、サウンド、物理ロジックを統合し、リッチなライティング、影、カメラワーク、ポストプロセス（Juice演出）を実装。
   - 画面上にHPゲージや照準レティクル、スコア等の2DオーバーレイHUDを配置し、`src/main.ts` を構成。

### Phase 6: QA・自律検証＆自動修復ループ
5. **`game-3d-qa-specialist` を起動**:
   - ビルド（`npm run build`）および型チェック（`tsc --noEmit`）を実行。
   - FAILの場合は原因カテゴリ（ASSET / AUDIO / LOGIC / SCENE）を特定し、該当サブエージェントへ修正指示を再ディスパッチ（最大3回ループ）。
   - PASSするまで自律修復を継続。

### Phase 7: ドキュメント作成 & 完成引き継ぎ
- `README.md` にゲームの遊び方、カメラ操作キー、パラメータ（`constants.ts`）の調整方法を記載して完了報告。

---

## 標準コンテキスト伝達フォーマット (`<SUBAGENT_CONTEXT>`)

各サブエージェント起動時は、以下のMarkdownブロックをプロンプト冒頭に付与してください：

```markdown
<SUBAGENT_CONTEXT>
- **対象ゲーム名**: {ゲームタイトル}
- **参照仕様書**: docs/gdd.md
- **担当スペシャリスト**: {サブエージェント名}
- **3Dエンジン構成**: Three.js + Cannon-es + Vite + TypeScript
- **前提条件・参照成果物**:
  - 3Dアセット定義: src/assets/asset-manifest.ts
  - 3Dサウンド定義: src/audio/sound-manager.ts
  - 3D物理・ロジック: src/entities/ & src/systems/
- **実装対象ファイル**: {ファイル一覧}
- **品質規約**: プレースホルダーの禁止 / 3D Juice演出の徹底 / TypeScript Strict準拠
</SUBAGENT_CONTEXT>
```
