# 任務：HC AOP 拆解與說明文件

## 背景
整理 FY__ Headcount AOP，產出每個 Gen / Project 的預估數字與計算邏輯，
供主管向 BU 代表說明。

## 口徑定義（不可更動）
- 單位：HC，以 man-quarter 計，粒度為 Project × 季（Q1–Q4）
- 階層：BG → BU → Gen → Project
- 原始總數位置：[檔名] / [Sheet] / [儲存格]，定義為 [全年man-quarter總和 / 年平均HC / Q4人數]
- 數字分兩層：直接估算層、分攤層（資源池 × 分攤比例）

## 輸入檔案
- Excel：[路徑]（主要數字來源）
- 計算公式與邏輯說明：[路徑]
- 逐字稿：[路徑]（主要理由來源）
- PPT：[路徑]

## 鐵則
1. 所有數字只能來自檔案，計算一律用 Python 執行，禁止心算或估算。
2. 每個數字附來源：檔名 / Sheet / 儲存格 / 公式原文。
3. 每個理由附來源：逐字稿檔名 + 時間戳 + 發言者，或 PPT 頁碼。
4. 找不到依據時寫「未找到依據」，禁止推論補齊。
5. 來源衝突時並列各方說法與出處，標記「待確認」，不自行裁決。
6. 來源優先序：數字 Excel公式 > Excel值 > PPT > 逐字稿；理由 逐字稿 / PPT 為主。

## Excel 處理方式
- 用 openpyxl 分別讀取 data_only=False（公式）與 data_only=True（快取值）。
- 從總數儲存格往回追公式相依樹，展開到 driver 為止。
- 用 Python 依公式重算，並與快取值比對。
- 標記：寫死數字（plug）、外部連結、隱藏 sheet、named range、錯誤值。
- 不要把整份 Excel 內容印進對話，只輸出結構摘要與必要片段。

## 執行階段（每階段結束後停下，等我確認）
- Phase 0 盤點：檔案清單、sheet 結構、總數位置、Gen/Project 清單、資源池清單
- Phase 1 抽數：輸出 master.csv（欄位見下）
- Phase 2 對帳：總數、by BU、by Gen、by 季、分攤守恆檢查
- Phase 3 補邏輯：對應逐字稿 / PPT 理由
- Phase 4 產出：A 稽核底稿、B Project 說明卡、C 主管總覽

## master.csv 欄位
BU | Gen | Project | 季度 | 類型(直接/分攤) | 資源池 | 分攤比例 | HC |
計算式(白話) | Excel公式原文 | 來源儲存格 | Driver值 | 理由摘要 | 理由出處 |
狀態(已確認/待確認/無依據)

## 驗收標準
- [ ] 所有 Project 加總 = 原始總數（誤差 0，捨入規則需說明）
- [ ] by BU / by Gen / by 季小計與 Excel 一致
- [ ] 每個資源池分攤比例合計 = 100%，分攤前後總數不變
- [ ] Python 重算結果 = Excel 快取值
- [ ] master.csv 每列皆有來源欄位
- [ ] 產出待確認清單（plug、衝突、無依據）
