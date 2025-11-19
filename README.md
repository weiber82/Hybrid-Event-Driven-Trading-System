Hybrid Event-Driven Trading System using XGBoost and Large Language Models
基於 XGBoost 事件偵測與大型語言模型語義推理之混合式盤中交易決策系統
一、專案總覽（Overview）

本專案建立一套事件驅動（Event-Driven）的盤中交易決策系統。
系統結合傳統機器學習與大型語言模型（LLM），提升訊號偵測能力、解釋性與決策穩定性。

系統包含三個模組：

L0：XGBoost 事件偵測

L1：SHAP 語義轉譯

L2：LLM 決策代理人

二、專案資料夾結構（Repository Structure）
├── data/
│   ├── raw/                 # 原始 Tick / K 線資料
│   ├── processed/           # 前處理與特徵工程後的資料
│   └── labels/              # Triple-Barrier 標註結果
│
├── docs/
│   ├── literature/          # 文獻整理、筆記
│   ├── methodology/         # 架構圖、公式推導、方法說明
│   ├── experiments/         # 測試與實驗紀錄
│   └── thesis/              # 論文內容（LaTeX / Markdown）
│
├── src/
│   ├── L0_detect/           # XGBoost 訓練 + Triple-Barrier 標註
│   ├── L1_semantic/         # SHAP-to-text 語義生成邏輯
│   ├── L2_agent/            # LLM 推理與決策模組
│   ├── features/            # 技術指標 & 微結構特徵
│   └── backtest/            # 回測引擎與績效計算
│
├── experiments/             # 可重現的實驗資料與紀錄
│
├── requirements.txt
└── README.md

三、系統架構（System Workflow）

整體資料流程為：

從市場取得 Tick 或 K 棒資料

L0：利用 XGBoost 進行事件偵測

L1：使用 SHAP 特徵重要度轉換成語義描述

L2：LLM 依據語義摘要與市場背景進行決策

產生最終交易動作（BUY / SELL / NO_ACTION）

四、L0：事件偵測層（XGBoost + Triple Barrier）
功能

使用 XGBoost 多分類模型

透過 Triple-Barrier Method 製作資料標註

使用技術指標與微結構特徵

RSI、MACD、Bollinger Bands

Volume Imbalance

Volatility Regime

滾動視窗統計特徵

L0 輸出

類別（0 = 中性，1 = 做多，2 = 做空）

類別機率

SHAP 特徵重要度

五、L1：語義轉譯層（SHAP-to-Text）

此層將 L0 的決策依據轉換為可讀的語義摘要。

範例輸出
訊號：LONG
關鍵因素：
- Volume imbalance +0.47（買盤力量偏強）
- RSI 從 35 上升至 48（短期動能回升）
- 價格站上 Bollinger 中線
市場狀態：高波動、偏多

六、L2：LLM 決策代理人（Decision Agent）

LLM 會根據 L0 與 L1 資訊進行二階推理，內容包含：

趨勢判斷

假訊號過濾

市場背景比對

風險評估

輸出格式
動作：BUY
信心度：0.82
理由：
- 訊號與市場中期趨勢一致
- 買盤力量持續增強
- 微結構指標未出現反向訊號

七、研究目標（Research Objectives）

Hybrid 架構是否優於純 ML 或純 LLM？

SHAP-to-Text 是否能反映真實市場微結構？

LLM 在相似情境下是否具備決策一致性？

八、文獻搜尋建議

推薦搜尋關鍵字：

triple barrier method

event-driven trading

SHAP explainability finance

LLM trading agent

hybrid intelligence

market microstructure

九、使用方式（Getting Started）
安裝套件
pip install -r requirements.txt

訓練 L0 模型
python src/L0_detect/train_xgb.py

執行 L1 語義生成
python src/L1_semantic/generate_summary.py

執行 L2 決策代理人
python src/L2_agent/run_agent.py

十、回測（Backtesting）

回測模組位於：

src/backtest/


提供：

Hit Rate

Sharpe Ratio

Max Drawdown

Precision / Recall