# Morphogenesis: Mathematical Chaos & Fractal Archive v2.2

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-2.2-green.svg)
![Technology](https://img.shields.io/badge/tech-HTML5%20%7C%20Canvas%20API%20%7C%20TailwindCSS-00f2ff.svg)

**[Live Demo Launch (GitHub Pages)](https://funmatu.github.io/morphogenesis-chaos-archive/)**

---

## 📑 目次 (Table of Contents)

1.  [プロジェクト概要 (Overview)](#1-プロジェクト概要-overview)
2.  [数理モデルとアルゴリズム詳細 (Mathematical Models)](#2-数理モデルとアルゴリズム詳細-mathematical-models)
    *   [Lorenz Attractor](#21-lorenz-attractor-ローレンツアトラクタ)
    *   [Aizawa Attractor](#22-aizawa-attractor-相澤アトラクタ)
    *   [Rössler Attractor](#23-rössler-attractor-レスラーアトラクタ)
    *   [Clifford Attractor](#24-clifford-attractor-クリフォードアトラクタ)
    *   [Mandelbrot Set](#25-mandelbrot-set-マンデルブロ集合)
    *   [Barnsley Fern](#26-barnsley-fern-バーンズリーのシダ)
    *   [Langton's Ant](#27-langtons-ant-ラングトンのアリ)
3.  [TDA (トポロジカルデータ分析) エンジン](#3-tda-トポロジカルデータ分析-エンジン)
    *   [算出メトリクス](#算出メトリクス)
    *   [Persistence Index (ホモロジー的持続性) の実装](#persistence-index-ホモロジー的持続性-の実装)
4.  [技術アーキテクチャ (Technical Architecture)](#4-技術アーキテクチャ-technical-architecture)
    *   [レンダリングパイプライン](#レンダリングパイプライン)
    *   [状態管理とパラメータ制御](#状態管理とパラメータ制御)
    *   [3D投影ロジック](#3d投影ロジック)
5.  [UI/UX デザインシステム](#5-uiux-デザインシステム)
6.  [インストールと使用方法](#6-インストールと使用方法)
7.  [カスタマイズガイド](#7-カスタマイズガイド)
8.  [ライセンス](#8-ライセンス)

---

## 1. プロジェクト概要 (Overview)

**Morphogenesis v2.2** は、複雑系科学における数理モデルをリアルタイムでシミュレーションし、その「形態形成（Morphogenesis）」のプロセスを可視化するWebベースの実験室です。

本システムは単なる可視化にとどまらず、生成された点群データに対してリアルタイムに簡易的な **トポロジカルデータ分析（TDA: Topological Data Analysis）** を実行し、カオスの構造的安定性や複雑性を定量評価する機能を備えています。

### 主な特徴
*   **高パフォーマンス描画**: HTML5 Canvas APIを直接操作し、数万〜数百万のパーティクル計算を60fpsで処理。
*   **多様な数理モデル**: 連続力学系（常微分方程式）、離散力学系（写像）、フラクタル幾何学、セル・オートマトンを網羅。
*   **リアルタイムTDA**: シミュレーション実行中に動的に変化するトポロジー（位相幾何学的性質）を数値化。
*   **インタラクティブ制御**: 各モデルの係数やパラメータをスライダーで即座に変更し、バタフライ効果や相転移を観察可能。
*   **依存関係なし**: 外部ライブラリとしてTailwind CSS（CDN）のみを使用し、コアロジックは完全なVanilla JavaScriptで構築。

---

## 2. 数理モデルとアルゴリズム詳細 (Mathematical Models)

本アーカイブに実装されている各シミュレーションの数学的定義と実装詳細は以下の通りです。

### 2.1. Lorenz Attractor (ローレンツアトラクタ)
気象学者エドワード・ローレンツが提示した、カオス理論の金字塔となる3次元非線形常微分方程式系です。

*   **数式定義**:
  
$$
    \begin{cases}
    \frac{dx}{dt} = \sigma (y - x) \\
    \frac{dy}{dt} = x (\rho - z) - y \\
    \frac{dz}{dt} = xy - \beta z
    \end{cases}
$$
    
*   **実装パラメータ**:
    *   $\sigma$ (Sigma): プラントル数。流体の粘性に関連。
    *   $\rho$ (Rho): レイリー数。熱輸送の指標。初期値28付近でストレンジアトラクタが発生。
    *   $\beta$ (Beta): 領域の幾何学的特性。
*   **挙動**: 決定論的でありながら予測不可能。初期値鋭敏性（バタフライ効果）の最も著名な例。

### 2.2. Aizawa Attractor (相澤アトラクタ)
球対称性を持ち、Z軸を貫くチューブのような構造を持つ美しいアトラクタです。

*   **数式定義** (コード内実装):

$$
    \begin{cases}
    \frac{dx}{dt} = (z - b)x - dy \\
    \frac{dy}{dt} = dx + (z - b)y \\
    \frac{dz}{dt} = c + az - \frac{z^3}{3} - (x^2 + y^2)(1 + ez) + f z x^3
    \end{cases}
$$
    
*   **特徴**: 非常に複雑な多項式を含み、パラメータ $e, f$ によって軌道の「ねじれ」や球体への巻き付き方が劇的に変化します。

### 2.3. Rössler Attractor (レスラーアトラクタ)
オットー・レスラーによって記述された、ローレンツ方程式より単純化された系ですが、位相幾何学的には「リボンの折り畳み（Folding）」というカオスの基本特性を明確に示します。

*   **数式定義**:

$$
    \begin{cases}
    \frac{dx}{dt} = -y - z \\
    \frac{dy}{dt} = x + ay \\
    \frac{dz}{dt} = b + z(x - c)
    \end{cases}
$$
    
*   **特徴**: XY平面上では螺旋を描き、Z軸方向へ跳ね上がる（サージ）動作を繰り返します。周期倍分岐の観察に最適です。

### 2.4. Clifford Attractor (クリフォードアトラクタ)
2次元平面上の離散力学系（写像）です。三角関数を用いた単純な反復から、有機的なテクスチャのようなパターンが生成されます。

*   **数式定義**:

$$
    \begin{cases}
    x_{n+1} = \sin(a y_n) + c \cos(a x_n) \\
    y_{n+1} = \sin(b x_n) + d \cos(b y_n)
    \end{cases}
$$
    
*   **実装詳細**: 累積描画を行うことで密度分布を可視化しています。パラメータの微細な変化が、点群のトポロジーを劇的に変化させます。

### 2.5. Mandelbrot Set (マンデルブロ集合)
複素平面上におけるフラクタルの王様です。

*   **数式定義**:

$$ z_{n+1} = z_n^2 + c $$
    
ここで $z, c$ は複素数。 $z_0 = 0$ から開始し、発散しない $c$ の集合。

*   **実装**: キャンバスのピクセルごとに反復計算を行い、発散速度に応じた色付け（HSL色空間）を行っています。無限の解像度と自己相似性を持ちます。

### 2.6. Barnsley Fern (バーンズリーのシダ)
反復関数系（IFS: Iterated Function System）を用いたフラクタルの例です。確率的なアフィン変換の組み合わせにより、自然界のシダ植物に酷似した形状が出現します。

*   **アルゴリズム**:
    4つの変換ルールを特定の確率で選択し、点を移動させます。
    1.  茎 (1%)
    2.  葉全体 (85%)
    3.  左の小葉 (7%)
    4.  右の小葉 (7%)

### 2.7. Langton's Ant (ラングトンのアリ)
単純なルールから複雑な挙動が創発する2次元チューリングマシン（セル・オートマトン）です。

*   **ルール**:
    *   白いマスにいる場合: 90度右回転し、色を反転させ、1マス進む。
    *   黒いマスにいる場合: 90度左回転し、色を反転させ、1マス進む。
*   **フェーズ**:
    1.  単純期 (Simplicity)
    2.  カオス期 (Chaos): 本システムでの可視化対象。
    3.  ハイウェイ期 (Highway): 10,000ステップ前後で出現する周期的動作。

---

## 3. TDA (トポロジカルデータ分析) エンジン

本システムには、生成されたデータの幾何学的・位相的構造をリアルタイムで定量化する独自の軽量TDAモジュールが組み込まれています。

### 算出メトリクス

1.  **Metric A (Orbit Volume / Active Cells)**
    *   **定義**: アトラクタの場合、点群を内包するバウンディングボックスの体積 ($dx \times dy \times dz$)。アリの場合、アクティブなセルの総数。
    *   **意味**: システムが位相空間内で占める領域の大きさ、あるいはエントロピーの大きさを示唆します。

2.  **Metric B (Spatial Spread)**
    *   **定義**: 点群の対角線距離（ユークリッド距離） $\sqrt{dx^2 + dy^2 + dz^2}$。
    *   **意味**: アトラクタの空間的な広がり、発散度合いを示します。

3.  **Persistence Index (TDA Persistence)**
    *   **概要**: パーシステントホモロジー（Persistent Homology）の概念を簡易的に応用した指標。
    *   **アルゴリズム**:
        1.  現在の点群からサンプリングを行う（計算コスト削減のため）。
        2.  点群全体の広がりの5%を閾値 $\epsilon$ (イプシロン) として設定。
        3.  点と点の距離が $\epsilon$ 未満であれば「接続されている（Connected）」とみなし、簡易的なリープ・グラフ（Rips Graph）の接続密度を計算。
    *   **解釈**:
        *   **High Value**: 構造が密で、安定したトポロジー（塊や明確な線）を形成している。
        *   **Low Value**: 点が散逸しており、カオス的またはノイズに近い状態。
    *   **目的**: 視覚的な「形」の安定性を数値化し、パラメータ変化による構造崩壊（Bifurcation）を検知します。

---

## 4. 技術アーキテクチャ (Technical Architecture)

### レンダリングパイプライン
`requestAnimationFrame` を用いたメインループ内で以下の処理を行っています。

1.  **Physics Update**: 各シミュレーションの `update()` 関数を実行。微分方程式の数値積分（オイラー法近似）や離散写像の計算。
2.  **Metric Calculation**: 新しい状態ベクトルに基づき、TDA指標を再計算。
3.  **Draw**: Canvas 2D Context APIを使用。
    *   **Trail Effect**: アトラクタ描画時、`globalCompositeOperation = 'lighter'` (加算合成) を使用し、軌跡が重なる部分を光らせる表現を実装。
    *   **Fade Effect**: 画面全体を低い不透明度の黒で塗りつぶすことで、過去の軌跡が徐々に消える残像効果を実現。

### 状態管理とパラメータ制御
*   `simDefinitions` オブジェクト内に各シミュレーションの `init`, `update`, `draw` メソッド、およびパラメータ初期値をカプセル化。
*   Strategy Patternに近い設計により、新しいシミュレーションの追加が容易です。
*   HTML Input Range要素の変更イベントをリッスンし、動的に物理定数を書き換えます。

### 3D投影ロジック
3次元座標 $(x, y, z)$ を2次元スクリーン $(x_{2d}, y_{2d})$ に投影するために、簡易的な回転行列を使用しています。

```javascript
// 簡易実装の疑似コード
x1 = x * cos(ry) - z * sin(ry);
z1 = x * sin(ry) + z * cos(ry);
y2 = y * cos(rx) - z1 * sin(rx);
// 遠近法（パースペクティブ）は意図的に除外し、平行投影に近い形で構造を明確化
```

---

## 5. UI/UX デザインシステム

*   **Glassmorphism**: 半透明のパネル（`backdrop-filter: blur`）を採用し、背後のシミュレーションを隠さずに情報を表示。
*   **Typography**:
    *   見出し: `Space Grotesk` (幾何学的で未来的なサンセリフ)
    *   データ・数値: `JetBrains Mono` (開発者向けの等幅フォント)
*   **Color Palette**:
    *   Background: Deep Black (`#050505`)
    *   Accent: Cyan (`#00f2ff`) 〜 Purple (`#7000ff`) のグラデーション
    *   Text: High contrast Gray (`#e0e0e0`)

---

## 6. インストールと使用方法

本プロジェクトはビルドプロセスを必要とせず、ブラウザだけで動作します。

1.  **クローン**:
    ```bash
    git clone https://github.com/funmatu/morphogenesis-chaos-archive.git
    ```
2.  **実行**:
    ディレクトリ内の `index.html` を最新のWebブラウザ（Chrome, Firefox, Edge, Safari）で開いてください。
    ※ ローカルサーバー（VS CodeのLive Server等）経由での起動を推奨しますが、直接ファイルを開いても動作します。

---

## 7. カスタマイズガイド

新しいアトラクタを追加したい場合は、`simDefinitions` オブジェクトに以下のようなエントリを追加してください。

```javascript
newSimName: {
    title: "New Attractor Name",
    desc: "Description here...",
    type: 'continuous', // or 'discrete'
    init() {
        // パラメータ初期化
        params = { a: 10, b: 28, dt: 0.01, size: 3000, color: '#ff0000' };
        points = [{ x: 1, y: 1, z: 1 }];
        view.zoom = 10;
    },
    update() {
        // 数理計算ロジック
        // points配列へのpushとshift
        updateMetrics(points);
    },
    draw() {
        // Canvas描画ロジック
    }
}
```
その後、HTMLの `#simSelector` 内にボタンを追加することでUIに反映されます。

---

## 8. ライセンス

このプロジェクトは **MIT License** の下で公開されています。

Copyright (c) 2025 Funmatu
