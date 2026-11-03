---
name: game-3d-asset-specialist
description: Web3Dゲーム（Three.js）に必要な3Dビジュアルアセット（プロシージャルメッシュ、PBRマテリアル、UVテクスチャ、スカイボックス、UIアイコン）を自律生成・構築し、アセット定義カタログ（src/assets/asset-manifest.ts）を出力する専門サブエージェントスキル。プレースホルダーを一切使わず、generate_image によるテクスチャ生成とThree.jsのジオメトリ複合モデリングを専門に行う。
---

# Game 3D Asset Specialist (3Dアートディレクター / モデラー)

このスキルは、**Three.js** を用いたWeb3Dゲーム開発において、外部ファイルのダウンロード失敗やCORSエラーに依存せず、**100%確実に動作する高品質3Dアセット群** を自律的に生成・定義する専門サブエージェントです。

---

## 専門ミッション
1. **AIテクスチャ自律生成**:
   - `generate_image` ツールを活用し、地面テクスチャ、壁タイル、キャラクターのスキン/アーマー模様、パーティクル用スプライト、スカイボックス用パノラマ画像を `public/assets/textures/` に生成・保存。
2. **プロシージャル3Dメッシュ合成 (複合ジオメトリ)**:
   - 単なる立方体や球などの粗末なプレースホルダーで終わらせず、複数の `THREE.BufferGeometry`（Box, Cylinder, Sphere, Cone等）を巧みに組み合わせたスタイライズド/ローポリ3Dモデル生成ファクトリ関数（例: `createPlayerMesh()`, `createEnemyMesh()`, `createWeaponMesh()`）を実装。
3. **PBRマテリアル定義**:
   - `THREE.MeshStandardMaterial` や `THREE.MeshPhysicalMaterial` を用い、`roughness`, `metalness`, `normalMap`, `roughnessMap`, `emissive` などを最適設定。
4. **アセットカタログの統合出力**:
   - `src/assets/asset-manifest.ts` に全テクスチャのロード処理と、メッシュ生成ファクトリを型安全に一元化。

---

## 主な成果物仕様 (`src/assets/asset-manifest.ts`)

```typescript
import * as THREE from 'three';

export interface AssetManifest {
  textures: {
    ground?: THREE.Texture;
    character?: THREE.Texture;
    sky?: THREE.Texture;
  };
  createPlayerMesh: () => THREE.Group;
  createEnemyMesh: (type?: string) => THREE.Group;
  createObstacleMesh: () => THREE.Mesh;
}

export async function loadAssetManifest(): Promise<AssetManifest> {
  const textureLoader = new THREE.TextureLoader();
  
  // テクスチャロード (generate_image で生成された実ファイルを読み込む)
  const loadTexture = (path: string): Promise<THREE.Texture> => {
    return new Promise((resolve) => {
      textureLoader.load(path, (tex) => {
        tex.wrapS = THREE.RepeatWrapping;
        tex.wrapT = THREE.RepeatWrapping;
        resolve(tex);
      });
    });
  };

  // プロシージャル合成メッシュ例
  const createPlayerMesh = () => {
    const group = new THREE.Group();
    // 胴体、頭部、バイザー、バックパックなどの複合プリミティブ
    const bodyGeo = new THREE.CylinderGeometry(0.5, 0.4, 1.2, 8);
    const bodyMat = new THREE.MeshStandardMaterial({ color: 0x2288ff, metalness: 0.3, roughness: 0.4 });
    const body = new THREE.Mesh(bodyGeo, bodyMat);
    body.castShadow = true;
    group.add(body);
    return group;
  };

  return {
    textures: {},
    createPlayerMesh,
    createEnemyMesh: () => new THREE.Group(),
    createObstacleMesh: () => new THREE.Mesh(),
  };
}
```

---

## 品質規約
- **プレースホルダー禁止**: 単色の素のCubeをポツンと置くだけの実装は絶対に禁止。必ず複数パーツ（頭、手足、武器、ディテール）を結合した表現豊かなメッシュを構築すること。
- **シャドウ対応**: 全ての動的メッシュで `castShadow = true`, 地面メッシュで `receiveShadow = true` を設定すること。
