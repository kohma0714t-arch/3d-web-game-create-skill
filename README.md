# 3D Web Game Create Skill 

Google Antigravity 向けの自律型 3D Webブラウザゲーム制作エージェントスキルです。
Three.js、Cannon-es（または Rapier3D）、Vite、TypeScript を主軸とし、親オーケストレーターと5つの専門職仕様（`references/`）が協調して、企画立案からアセット・音響・ロジック・演出・ビルド検証までをフル自律で開発します。

本スキルは Antigravity のスラッシュコマンド（`/`）一覧を汚さないよう、専門サブエージェント仕様を親スキルの `references/` 配下に完全統合・集約したクリーンな設計となっています。

---

##  特徴

- **スラッシュコマンド最適化**: 親コマンド（`/web-3d-game-create`）のみがチャットの `/` に候補表示され、サブエージェント仕様は内部の `references/` から自動読込されるため、UIの邪魔になりません。
- **フル自律パイプライン**: ユーザーの簡単な要望（「三人称の3Dアクションゲームを作って」「宇宙を飛ぶ3Dシューティングを作って」など）から、3D-GDD策定、実装、テスト検証までを一貫して自動化。
- **外部ファイル不要のプロシージャル設計**:
  - **ビジュアル**: `generate_image` を活用したテクスチャ生成と Three.js プリミティブ複合モデリングによるリッチな3D表現。
  - **オーディオ**: Web Audio API を活用したプロシージャル立体音響（PositionalAudio）＆ BGMシンセサイザー。
- **堅牢な物理＆描画アーキテクチャ**: Cannon-es / Rapier3D 物理演算と Three.js の同期、三人称追従カメラ、ライティング/シャドウ、2DオーバーレイHUD。
- **自己修復QAループ**: ViteビルドおよびTypeScript型チェックを自律実行し、エラーがあれば特定サブエージェントへ差し戻して自動修正。

---

##  ディレクトリ構成

```text
3d-web-game-create-skill/
├── docs/
│   └── web_3d_game_create_architecture.md # アーキテクチャ詳細設計書
└── skills/
    └── web-3d-game-create/                #  親オーケストレータースキル（単一スキルとして導入可能）
        ├── SKILL.md                       # 親エージェント指示書
        └── references/                    # 専門サブエージェント仕様定義
            ├── game-3d-asset-specialist.md   #  3Dビジュアル・テクスチャ専門仕様
            ├── game-3d-audio-specialist.md   #  3D立体音響・Web Audioシンセ専門仕様
            ├── game-3d-logic-specialist.md   #  3D物理・エンティティ・ゲームループ専門仕様
            ├── game-3d-scene-specialist.md   #  Three.jsシーン・カメラ・描画・UI専門仕様
            └── game-3d-qa-specialist.md      #  ビルド検証・整合性修復専門仕様
```

---

##  専門職仕様一覧

| 専門仕様（references） | 担当フェーズ | 主要技術 / 担当領域 |
|---|---|---|
| **game-3d-asset-specialist.md** | 3Dビジュアル生成 | Three.js マテリアル/プロシージャルメッシュ、UVテクスチャ生成、`asset-manifest.ts` |
| **game-3d-audio-specialist.md** | 3D音響・BGM生成 | Web Audio API、Three.js `PositionalAudio`、シンセサイザーSE/BGM、`sound-manager.ts` |
| **game-3d-logic-specialist.md** | 3D物理・ゲームロジック | Cannon-es / Rapier3D 物理同期、キャラクター操作、衝突判定、`src/entities/*` |
| **game-3d-scene-specialist.md** | 画面演出・シーン統合 | レンダラー、三人称/一人称追従カメラ、ライティング、HUDオーバーレイ、エントリーポイント `src/main.ts` |
| **game-3d-qa-specialist.md** | 品質検証・自動修復 | `npm run build`、`tsc --noEmit`、Three.js メモリ解放・リソース整合性検証と自動修復 |

---

##  導入方法

Antigravity のスキルディレクトリ（ユーザー環境の `~/.gemini/config/skills/`）に本リポジトリの `skills/web-3d-game-create` フォルダをそのまま配置するだけで利用可能です。

### Windows の場合 (PowerShell):
```powershell
Copy-Item -Path "skills\web-3d-game-create" -Destination "$env:USERPROFILE\.gemini\config\skills\" -Recurse -Force
```

### macOS / Linux の場合:
```bash
cp -r skills/web-3d-game-create ~/.gemini/config/skills/
```

---

##  使い方

Antigravity のチャット上で、以下のように呼び出します：

```text
/web-3d-game-create 宇宙空間を舞台にした3Dドッグファイトアクションゲームを作りたい
```

または：

```text
Three.jsでサイバーパンク風の3D障害物ランゲームを作成してください
```

親エージェントが自動的に起動し、企画ヒアリングから各専門仕様を読み込んで自律的にゲームを完成させます。

---

##  詳細ドキュメント

システムの全体アーキテクチャや各エージェントのインターフェース仕様については、[docs/web_3d_game_create_architecture.md](docs/web_3d_game_create_architecture.md) を参照してください。

---

## 📄 ライセンス

MIT License
