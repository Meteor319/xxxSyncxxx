# Role & Context
你現在是一位頂尖的資料視覺化架構師（Data Visualization Architect）與營運管理系統專家。
我們目前正在為公司的營運管理辦公室設計一套全新的「RAS (Resource Allocation System / 資源配置系統)」與「M Time (Man-Time / 工時)」視覺化儀表板。
我希望這套系統的 UI/UX 與數據呈現邏輯，能夠完美借鑑「Apple 睡眠分析 (Apple Sleep Analysis)」的核心精神：將連續且複雜的時間序列數據，轉化為直覺的狀態深度與資源消耗指標。

# Task
請根據我提供的【Apple 睡眠設計邏輯與 RAS/M Time 的對應關係】，為我產出一份「詳細且無所遺漏」的儀表板設計規格書。這份規格書需要能直接交給前端工程師（或用於 Tableau 開發）與後端資料庫團隊進行實作。

# Mapping Concept (Apple Sleep ➔ RAS & M Time)
1. 漸進式資訊揭露 (Progressive Disclosure)：
   - Apple 睡眠：從週/月的「平均睡眠時間」下鑽到單日的「睡眠階段」。
   - RAS 系統：從季/月的「專案資源達標率/稼動率」下鑽到單週/單日的「工程師 M Time 投入深度」。

2. 狀態與深度的視覺隱喻 (Sleep Stages ➔ M Time Depth)：
   - Awake (清醒/橘紅色) ➔ Overhead & 中斷 (如：行政庶務、臨時插單、跨部門協調)。
   - REM (淺眠/淺藍色) ➔ 協作與發散 (如：Code Review、架構會議)。
   - Core (核心/深藍色) ➔ 常規產出 (如：例行性開發、驗證執行)。
   - Deep (深層/深紫色) ➔ 深度工作 (如：解 Critical Bug、核心演算法設計、Tape-out 前衝刺)。

3. 虛實對比 (Capacity vs. Utilization)：
   - Apple 睡眠：在床時間 (淺色長條) vs. 實際睡眠 (深色長條) vs. 睡眠目標 (虛線)。
   - RAS 系統：總分配 M Time (Allocated Capacity) vs. 實際高價值產出 M Time (Effective Utilization) vs. 稼動率基準線。

4. 跨維度指標疊加 (Cross-Metric Overlay)：
   - Apple 睡眠：主時間軸疊加心率、呼吸頻率。
   - RAS 系統：M Time 時間軸疊加專案健康度指標（如：Issue 回報數量、CI/CD 失敗率、Bug 產生率）。

# Deliverables (請依序提供以下產出)

## 1. 儀表板版面架構設計 (Dashboard Layout & Flow)
- 詳細描述 Day / Week / Month / Quarter 不同時間維度下的視覺化圖表類型（請避免使用圓餅圖，多採用狀態甘特圖、堆疊面積圖或雙軸圖）。
- 定義使用者互動流程（如：Hover 顯示什麼？點擊展開什麼資料？）。

## 2. 資料結構定義 (Data Schema)
- 為了支撐上述視覺化，請使用 YAML 或 JSON 格式，定義這套儀表板所需要的底層資料結構（包含 User ID, Project Code, M Time Status, Vitals/Metrics 等欄位與資料型別）。

## 3. 異常狀態與主管洞察 (Management Insights & Alerts)
- 借鑑 Apple 睡眠的「呼吸中止通知」概念，請設計 3-5 個系統自動觸發的「營運健康度警報（Alerts）」。
- 舉例：當系統偵測到某專案的 "Deep Work" 極低，但 "Overhead" 與下方的 "Issue Rate" 飆高時，系統應跳出何種提示？對應的管理決策可能為何？

## 4. 前端實作建議
- 推薦適合用來繪製這種「多層次狀態時間軸 (類似 Hypnogram)」的開源圖表庫（如 ECharts, D3.js）或 Tableau 實作技巧。
