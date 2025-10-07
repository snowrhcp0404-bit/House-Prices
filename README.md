# House-Prices
## 🎯 目的
- **住宅価格（SalePrice）の予測精度を最大化**  
- **モデルの汎化性能と再現性を両立**  
- **特徴量エンジニアリングによる改善過程を体系的に整理**

---

## 📊 データ概要
- 出典: [Kaggle - House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)
- サンプル数: 約1460件（Train）
- 特徴量: 約80列（数値・カテゴリ混在）
- 目的変数: `SalePrice`（対数変換後を使用）

> ※ Kaggleの規約により、本リポジトリにはデータは含まれていません。  
> 各自Kaggleページからダウンロードしてください。

---

## ⚙️ アプローチ概要

### 🧹 1. 前処理（Preprocessing）
- 数値変数：中央値で補完  
- カテゴリ変数：`'None'` で補完  
- 外れ値処理：`GrLivArea`, `TotalBsmtSF`, `1stFlrSF` の上位外れ値を除去

### 🧮 2. 特徴量エンジニアリング
- 対数変換 (`log1p`)  
- カテゴリ×カテゴリ / カテゴリ×面積の交互作用特徴量  
- Target Encoding（KFoldベースでリーク防止）  
- 平均価格マッピング (`mean encoding`)  
- 分布差が大きい変数の削除（train/test drift対策）

### 🧠 3. モデル
| モデル | 内容 |
| CatBoost | カテゴリ変数自動処理・10-fold CV平均採用 |

### 🔁 4. 検証
- KFold (10-fold)  
- 評価指標：**RMSE（対数変換後）**  
- Early stoppingによる過学習抑制

---

## 🧪 結果

| モデル | mean fold RMSE(log) |OOF log RMSE| Public LB | Private LB |
| CatBoost | 0.1196| 0.12004 | **422位 / 4437チーム中（Top 10%）** |

> CatBoost単独モデルで **Private Leaderboard 422位 / 4437チーム中（Top 10%）** を達成。
