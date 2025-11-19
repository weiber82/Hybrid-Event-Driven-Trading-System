文獻導讀：Han, Kim & Enke (2023) 
https://drive.google.com/file/d/13rlO-YKE64VSXeCf6HM00ZN6wkNDARiZ/view?usp=drive_link

A Machine Learning Trading System for the Stock Market Based on N-Period Min-Max Labeling Using XGBoost
1. 研究背景與動機

多數交易模型依賴「Up-Down Labeling」進行漲跌分類，但此方法對微小價格波動極為敏感，使訓練資料充滿雜訊，造成模型難以穩定學習。
研究指出：

交易績效的關鍵不在模型複雜度，而在「標籤是否具備代表性」。

若標籤貼近真實市場事件，則可顯著提升模型品質與最終交易成果。

2. 研究目的

本研究提出 N-Period Min-Max (NPMM) Labeling，僅在「局部最低點」與「局部最高點」進行標註，以減少噪聲並提高事件品質。並透過 XGBoost 建立完整交易系統以驗證此標籤方法的有效性。

3. 研究方法與架構
3.1 Phase 1：資料構建

提取多種技術指標（RSI、ROC、STOCH、ADX、CCI 等）。

使用 NPMM Labeling 取代 Up-Down Labeling。

NPMM 僅在極值點標記 Up / Down（長線看 N=61、短線看 N=3）。

3.2 Phase 2：模型訓練

以 XGBoost 作為分類模型。

模型輸出 Up/Down 機率值，並以閾值 0.5 做二元分類。

3.3 Phase 3：交易模擬

根據模型輸出產生 Buy / Sell 訊號。

使用 open price 進行買賣。

評估指標包含：

Winning Ratio

Payoff Ratio（平均獲利 / 平均虧損）

Profit Factor（總獲利 / 總虧損）

4. 實驗數據與流程

資料來源：92 檔 NASDAQ 成分股（2009–2020）。

使用 Time-Series Cross Validation（TSCV）。

測試集依 3 個月為一期共 12 期。

5. 研究結果
5.1 N 值對標籤品質的影響

N 越大 → 標籤越少 → 噪聲越低，但風險增加資料不足

盈利因子在 N=21～61 表現最佳

標籤品質比資料量更重要

5.2 與其他 labeling 方法比較
Labeling 方法	Profit Factor	Learning Data Size	結論
Up-Down	0.93	最大	雜訊多
NPV Labeling	1.13	少	表現穩定
Sezer Labeling	1.34	很少	高風險高波動
NPMM	1.19	少	最穩定 + 高效

研究結論：
選對標籤遠比選對模型更重要。

6. 研究貢獻

提出 NPMM 標籤，改善 Up-Down labeling 造成的雜訊問題。

提供「事件式標籤 → XGBoost → 回測」的完整交易系統。

顯示 instance selection（挑關鍵點）會明顯提升交易績效。

強調模型評估應聚焦 trading performance 而非純 Accuracy。

7. 研究限制與未來方向

限制：

僅採用日資料

特徵僅限技術指標

僅測試 NASDAQ 大型股

未來方向：

延伸至高頻資料

測試更多 labeling（事件觸發、多分類）

結合深度學習或混合模型架構

8. 總結

研究展示標籤的品質決定模型的最終交易績效。
NPMM 作為事件型標籤能有效降低噪聲，使 XGBoost 的交易結果更穩定，並證明「精準事件標籤優於全點貼標」。

9. Applicability to This System（可用於本系統的參考重點）
9.1 與本系統 L0（XGBoost 事件偵測層）高度一致

本研究核心是 事件式標籤（Min-Max）＋ XGBoost

L0 核心是 Triple-Barrier（事件標籤）＋ XGBoost

可以引用作為 L0 理論基礎。

9.2 強調「標籤比模型重要」

這篇強調的重點與 Triple-Barrier 一致：
代表市場事件的標籤 → 決定模型能否成功。

9.3 提供事件式 labeling 的實證驗證框架

NPMM 方法為 Triple-Barrier Method 提供強有力的文獻先例：

使用極值點標籤（與 barrier hit 本質相似）

只在特定事件點貼標，而非全點 data labeling

以 trading performance 衡量標籤品質

這些都可直接複製到你的 L0 部分。

9.4 完整的「資料 → 標籤 → XGBoost → 回測」流程可借鏡

本系統也會遵循同樣架構，這篇可作為技術流程參考