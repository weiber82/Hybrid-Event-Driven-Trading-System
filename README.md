Thesis Project: Hybrid Event-Driven Trading System via XGBoost and Large Language Models

碩士論文專案：基於 XGBoost 事件偵測與 LLM 語義推理之盤中交易決策架構

1. Abstract (摘要)

本研究提出一套整合 傳統機器學習 (Machine Learning) 與 大型語言模型 (Large Language Models, LLMs) 的混合式交易決策架構（Hybrid AI Architecture），旨在解決高頻金融資料中「雜訊極高、事件稀疏」以及「黑盒模型缺乏解釋性」之核心問題。

本系統採用仿生認知架構，模擬人類交易員的 System 1 (直覺) 與 System 2 (推理) 思維模式，分為三層結構：

L0 — Signal Layer (數值事件偵測)：採用 XGBoost 搭配 Triple-Barrier Method 進行事件驅動標註，負責在大量市場雜訊中捕捉高信度的非線性訊號。

L1 — Semantic Layer (語義轉譯)：利用 SHAP (SHapley Additive exPlanations) 將 L0 的數值特徵貢獻轉化為自然語言解釋，實現「數值—語義對齊 (Numerical-Semantic Alignment)」。

L2 — Decision Layer (LLM 決策代理)：部署 LLM 作為二階驗證者 (Meta-Labeler)，結合市場上下文進行語義推理與風險控管，輸出結構化的最終交易指令。

本研究貢獻在於提出一種通用的神經符號 (Neuro-symbolic) 金融預測框架，並驗證此設計能否有效降低偽陽性訊號 (False Positives) 並提升決策的一致性。

2. Literature Review (文獻探討)

本研究之架構設計建立於以下關鍵學術文獻之上：

2.1 L0: Event Detection & Labeling (事件偵測與標註)

模型效能：Zhang (2022) 指出，相較於傳統線性模型，XGBoost 在處理金融時間序列的非線性特徵與過濾雜訊上具有顯著優勢，特別適用於處理技術指標的高變動資料 [cite: Zhang & Li, 2022]。

標註策略：針對金融標籤雜訊問題，Han, Kim & Enke (2023) 的研究證實，採用事件驅動的標註方法（如 Barrier-based 或 N-Period Min-Max）優於傳統的 Up/Down Labeling，能顯著提升模型的獲利因子 (Profit Factor) 與訊號品質 [cite: Han et al., 2023]。

2.2 L1: Explainable AI & Semantic Translation (可解釋性與語義層)

局部解釋：Ergün (2023) 強調 XGBoost 原生特徵重要度存在偏差，而 SHAP (TreeSHAP) 能提供更精確的局部解釋 (Local Explanation)，揭示單一預測背後的特徵貢獻邏輯 [cite: Ergün, 2023]。

語義轉譯：Burton et al. (2023) 提出將數值解釋轉換為自然語言 (Textual Explanations) 的框架，證實將特徵重要度「線性化 (Linearization)」為敘述性文本，能有效輔助後續的認知決策系統 [cite: Burton et al., 2023]。

2.3 L2: LLM Decision Agent (決策代理人)

預測有效性：Lopez-Lira & Tang (2023) 證實通用型 LLM 具備解讀財經資訊經濟意涵的能力，且其預測績效優於傳統情緒模型（如 BERT），支持本研究利用 LLM 進行訊號驗證的核心假設 [cite: Lopez-Lira & Tang, 2023]。

代理人架構：Yu et al. (2023) 的 FINMEM 研究證明，賦予 LLM 「角色設定 (Profiling)」 與 「記憶機制 (Memory)」 可顯著提升交易績效與適應性。本研究參考此架構設計 L2 Agent 的 System Prompt [cite: Yu et al., 2023]。

多代理協作：參考 Xiao et al. (2025) 的 TradingAgents 框架，本研究採用結構化溝通協定，確保數值模型 (L0) 與語言模型 (L2) 之間的資訊傳遞精確無誤 [cite: Xiao et al., 2025]。

3. System Architecture (系統架構)

L0 — XGBoost Event Detection Layer (事件偵測層)

Objective: 從雜訊極高的高頻資料中，過濾出具備顯著動能的市場狀態。

Data Source: NASDAQ 1-minute OHLCV (2013–2025).

Feature Engineering: RSI, MACD, ATR, Volume Ratio, Bias (乖離率).

Labeling Strategy: Triple-Barrier Method (Path-Dependent).

Horizon: 60 bars (1 hour).

Width: Dynamic based on 2.0x ATR.

Classes: Long (1) / Short (2) / Neutral (0).

Model: XGBoost Classifier with GPU acceleration.

L1 — SHAP Semantic Layer (語義轉譯層)

Objective: 將 L0 的數值預測結果「翻譯」為 LLM 可理解的語義 Prompt。

Mechanism:

SHAP Analysis: 計算當下預測的 Top-K 關鍵特徵貢獻值 (Feature Contribution)。

Linearization: 使用規則樣板 (Template-based) 將數值轉換為敘述。

Example: "RSI (+0.25) is the primary driver, indicating strong upward momentum."

L2 — LLM Decision Agent Layer (決策代理層)

Objective: 擔任二階驗證者 (Meta-Labeler)，進行風險審查與最終決策。

Role Definition: Senior Risk Manager (資深風控經理).

Reasoning Process: Chain-of-Thought (CoT) + Market Context Analysis.

Output Format (JSON):

{
  "action": "LONG",
  "confidence": 0.85,
  "reasoning": "The signal is supported by strong volume and RSI, aligning with the current uptrend."
}


4. Implementation Roadmap (實作路徑)

Phase 1 — L0 Core (The Foundation)

目標：建立穩健的事件偵測模型，並產出初步回測數據。

[ ] Data Preprocessing: 清洗 NASDAQ 分 K 資料，計算技術指標 (Pandas-TA)。

[ ] Labeling: 實作 Triple-Barrier Method (使用 Numba 加速)。

[ ] Modeling: 訓練 XGBoost 模型並進行 Walk-Forward Validation。

[ ] Baseline Eval: 評估 L0 單獨運作之 Precision/Recall。

Phase 2 — L1 Integration (The Bridge)

目標：打開黑盒模型，建立解釋性介面。

[ ] SHAP Analysis: 串接 TreeExplainer 計算特徵重要度。

[ ] Text Generation: 開發 shap_to_text 轉換函數 (Linearization)。

Phase 3 — L2 Agent (The Brain)

目標：整合 LLM 進行二階決策。

[ ] Agent Setup: 串接 OpenAI/Claude API，設定 System Prompt。

[ ] Experiment: 比較 XGBoost Only vs. XGBoost + LLM 的交易績效。

[ ] Case Study: 分析 LLM 成功過濾假訊號的具體案例。

5. File Structure

├── data/
│   ├── raw/                   # 原始 NASDAQ 1-min CSV
│   └── processed/             # 加工後的 Parquet/Feather 檔
├── docs/
│   ├── literature/            # 關鍵文獻導讀 (Lopez-Lira, Yu, Zhang, etc.)
│   └── methodology/           # 方法論筆記 (Triple-Barrier, SHAP 公式)
├── src/
│   ├── features/              # 技術指標計算模組
│   ├── labeling/              # Triple-Barrier Labeling (Numba)
│   ├── model/                 # XGBoost Training & Inference
│   ├── explain/               # L1: SHAP Analysis & Text Generation
│   └── agent/                 # L2: LLM API Client
├── notebooks/                 # Jupyter Notebooks for EDA & Experiments
├── requirements.txt
└── README.md
