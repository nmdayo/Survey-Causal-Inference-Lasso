## 概要
本リポジトリは、クラフトビール「白蒲田」のアンケートデータを対象に、**リピート意向に影響を与える真の要因**を特定するための分析コード群です。

単純な相関分析にとどまらず、**Lasso回帰による変数選択**、**順序ロジスティック回帰によるモデル化**、そして**傾向スコア（IPW法）を用いた因果推論**を組み合わせることで、交絡因子の影響を排除した因果効果（ATT）の推定を行っています。

## 分析フロー

本プロジェクトは大きく2つのフェーズで構成されています。

### Phase 1: データの構造探索
変数の関係性を可視化し、潜在的な構造を把握します。

* **`11_Phase1_Polychoric_Correlation.ipynb`**
    * 順序尺度（5段階評価）に適した**ポリコリック相関行列**を算出し、ヒートマップで可視化します。
* **`12_Phase1_Graphical_Lasso.ipynb`**
    * **Graphical Lasso** を用いて変数間の偏相関ネットワーク（無向グラフ）を推定し、直接的な結びつきを可視化します。
    * ベイズ最適化（Optuna）により正則化パラメータを自動調整しています。
* **`13_Phase1_Factor_Analysis.ipynb`**
    * 因子分析を行い、観測変数の背後にある潜在因子（味覚・視覚など）を確認します。

### Phase 2: 因果推論と効果検証
重要な変数を特定し、その因果効果を定量化します。

* **`21_Phase2_Lasso_Selection.ipynb`**
    * **Lasso回帰 (L1正則化)** を用いて、多重共線性を回避しつつ、リピート意向に寄与する重要な変数を客観的に選択（Feature Selection）します。
* **`22_Phase2_Ordinal_Logistic.ipynb`**
    * **順序ロジスティック回帰**を行い、選択された変数の統計的有意性を検定します。
    * AIC（赤池情報量基準）を用いてモデルの妥当性を評価します。
* **`23_Phase2_Causal_Inference_IPW.ipynb`**
    * **傾向スコア逆重み付け法 (IPW)** を用いて、主要因の**因果効果 (ATT: Average Treatment Effect on the Treated)** を推定します。
    * 共変量のバランスチェック（Love Plot）も行います。

## 使用データ
* `data.csv` (または `survey.csv`): アンケートのローデータ
* `correlation_matrix.csv`: Phase 1で算出された相関行列

## 環境構築

以下のライブラリを使用しています。

```bash
pip install pandas numpy matplotlib seaborn scikit-learn statsmodels factor_analyzer pydot optuna japanize-matplotlib
