# 3D Web Game Create Skill Suite 🎮✨

Google Antigravity 向けの自律型 3D Webブラウザゲーム制作エージェントスキル群です。
Three.js、Cannon-es（または Rapier3D）、Vite、TypeScript を主軸とし、親オーケストレーターと5つの専門サブエージェントが協調して、企画立案からアセット・音響・ロジック・演出・ビルド検証までをフル自律で開発します。

---

## 🌟 特徴

- **フル自律パイプライン**: ユーザーの簡単な要望（「三人称の3Dアクションゲームを作って」「宇宙を飛ぶ3Dシューティングを作って」など）から、3D-GDD策定、実装、テスト検証までを一貫して自動化。
- **外部ファイル不要のプロシージャル設計**:
  - **ビジュアル**: `generate_image` を活用したテクスチャ生成と Three.js プリミティブ複合モデリングによるリッチな3D表現。
  - **オーディオ**: Web Audio API を活用したプロシージャル立体音響（PositionalAudio）＆ BGMシンセサイザー。
- **堅牢な物理＆描画アーキテクチャ**: Cannon-es / Rapier3D 物理演算と Three.js の同期、三人称追従カメラ、ライティング/シャドウ、2DオーバーレイHUD。
- **自己修復QAループ**: ViteビルドおよびTypeScript型チェックを自律実行し、エラーがあれば特定サブエージェントへ差し戻して自動修正。

---

## 📂 ディレクトリ構成

```text
3d-web-game-create-skill/
├── docs/
│   └── web_3d_game_create_architecture.md # アーキテクチャ詳細設計書
└── skills/
    ├── web-3d-game-create/                # 👑 親オーケストレータースキル
    │   └── SKILL.md
    ├── game-3d-asset-specialist/          # 🎨 3Dビジュアル・テクスチャ専門スキル
    │   └── SKILL.md
    ├── game-3d-audio-specialist/          # 🔊 3D立体音響・Web Audioシンセ専門スキル
    │   └── SKILL.md
    ├── game-3d-logic-specialist/          # ⚙️ 3D物理・エンティティ・ゲームループ専門スキル
    │   └── SKILL.md
    ├── game-3d-scene-specialist/          # 🎬 Three.jsシーン・カメラ・描画・UI専門スキル
    │   └── SKILL.md
    └── game-3d-qa-specialist/             # 🛠️ ビルド検証・整合性修復専門スキル
        └── SKILL.md
```

---

## 🤖 構成エージェント一覧

| エージェント / スキル名 | 役割概要 | 主要技術 / 担当領域 |
|---|---|---|
| **web-3d-game-create** | 全体オーケストレーター | 3D-GDD策定、サブエージェントへのタスク分割と順次ディスパッチ、品質管理 |
| **game-3d-asset-specialist** | 3Dビジュアル生成 | Three.js マテリアル/プロシージャルメッシュ、UVテクスチャ生成、`asset-manifest.ts` |
| **game-3d-audio-specialist** | 3D音響・BGM生成 | Web Audio API、Three.js `PositionalAudio`、シンセサイザーSE/BGM、`sound-manager.ts` |
| **game-3d-logic-specialist** | 3D物理・ゲームロジック | Cannon-es / Rapier3D 物理同期、キャラクター操作、衝突判定、`src/entities/*` |
| **game-3d-scene-specialist** | 画面演出・シーン統合 | レンダラー、三人称/一人称追従カメラ、ライティング、HUDオーバーレイ、エントリーポイント `src/main.ts` |
| **game-3d-qa-specialist** | 品質検証・自動修復 | `npm run build`、`tsc --noEmit`、Three.js メモリ解放・リソース整合性検証と自動修復 |

---

## 🚀 導入方法

Antigravity のスキルディレクトリ（ユーザー環境の `~/.gemini/config/skills/`）に本リポジトリの `skills/` 配下の各スキルフォルダを配置することで利用可能になります。

### Windows の場合 (PowerShell):
```powershell
Copy-Item -Path "skills\*" -Destination "$env:USERPROFILE\.gemini\config\skills\" -Recurse -Force
```

### macOS / Linux の場合:
```bash
cp -r skills/* ~/.gemini/config/skills/
```

---

## 💬 使い方

Antigravity のチャット上で、以下のように呼び出します：

```text
/web-3d-game-create 宇宙空間を舞台にした3Dドッグファイトアクションゲームを作りたい
```

または：

```text
Three.jsでサイバーパンク風の3D障害物ランゲームを作成してください
```

親エージェントが自動的に起動し、企画ヒアリングから5つの専門エージェントを順次ディスパッチしてゲームを完成させます。

---

## 📖 詳細ドキュメント

システムの全体アーキテクチャや各エージェントのインターフェース仕様については、[docs/web_3d_game_create_architecture.md](docs/web_3d_game_create_architecture.md) を参照してください。

---

## 📄 ライセンス

MIT License
