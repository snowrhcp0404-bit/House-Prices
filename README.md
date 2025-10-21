# House-Prices
## 🎯 目的
- **住宅販売価格（SalePrice）の予測精度を最大化**  
- 特徴量エンジニアリングに重点を置き、スコア向上を図る。

---

## 📊 データ概要
- 住宅の属性情報や立地条件などをもとに住宅販売価格（SalePrice)を予測する回帰問題。
- 出典: [Kaggle - House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)
- サンプル数: 約1460件（Train）
- 特徴量: 約80列（数値・カテゴリ混在）
- 目的変数: `SalePrice`（対数変換後を使用）

> ※ Kaggleの規約により、本リポジトリにはデータは含まれていません。  
---

## ⚙️ アプローチ概要

### 🧹 1. 前処理（Preprocessing）
- 数値変数：中央値で補完（GarageYerBuiltをYearBuiltの中央値で補完）
- カテゴリ変数：`'None'` で補完  
- 外れ値処理：`GrLivArea`, `TotalBsmtSF`, `1stFlrSF` などの重要度の高い特徴量の外れ値を除去
- objectカラムをcategory型変換

### 🧮 2. 特徴量エンジニアリング
- 対数変換 (`log1p`)
- モデル重要度が高く、歪度と尖度の高い上位特徴量の対数変換
- カテゴリ×カテゴリ / カテゴリ×面積の交互作用特徴量
- 面積比や比率系特徴量作成（’1stFlrSF_ratio’, Per_GLA_Glなど）
- 二値化（’MiscVal_binary’,’Has3SsnPorch’など）
- Yeo-johnson変換（’BsmtFinSF1_yeo’） 
- 分布差が大きい変数の削除（train/test drift対策）
- モデル重要度の低い変数の削除

### 🧠 3. モデル
 Lightgbm, CatBoost
 Optunaで最適化
 カテゴリ変数自動処理・10-fold CV平均採用 

### 🔁 4. 検証
- KFold (10-fold)  
- 評価指標：**RMSE（対数変換後）**  
- Early stoppingによる過学習抑制

## 🧪 結果

| モデル | mean fold RMSE(log) |OOF log RMSE| Public LB | Private LB |
| CatBoost | 0.1196| 0.12004 | **422位 / 4437チーム中（Top 10%）** |

> CatBoost単独モデルで **Private Leaderboard 422位 / 4437チーム中（Top 10%）** を達成。
> 
## 総括
特徴量重要度の高い変数を軸に変数の取捨選択を行う。建築面積、延床面積、実質利用可能面積の大小が特に重要度に寄与することが判明。
歪度や尖度の大きさを確認し、欠損値や外れ値に対してアプローチ。
Optunaで最適化。カテゴリ変数に強いcatboostを利用し精度が向上した。
業務活用において、不動産価格の適正査定及び自動査定、市場動向の把握、販売戦略、投資判断を支援しうるモデル作成の知見を獲得。



