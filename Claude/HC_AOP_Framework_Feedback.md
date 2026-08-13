# 高階動態營運與資源談判框架 2.0 (Dynamic AOP & Resource Governance Framework)

## 1. 框架架構總覽 (Framework Architecture)

```
[ Phase 1: Strategic Alignment ] ──> [ Phase 2: Strategic Negotiation ] ──> [ Phase 3: Dynamic WPO Modeling ] ──> [ Phase 4: Execution & Governance ]
    頂層邊界與基線 (FIN/EO)            專案角力與防禦但書 (Corp vs. BG)        MTime/RAS 模型與 GM 挪格決策          MTime Dashboard 與 AI 稽核
```

### Phase 1: Strategic & Financial Alignment (頂層邊界與基線)
* **EO & FIN 節奏**：明確交付時間、格式、Review 流程與 Lesson Learned 調查。提供 Amort Ratio 與 BU Top-down Target。
* **底線基準**：以 RAS Q2 做為 Baseline，盤點新增 Delta。

### Phase 2: Strategic Negotiation & Defensive Strategy (戰略角力與防衛談判)
* **利益衝突調和 (Corp Level vs. BG Level)**：
  * **Corp Level Focus**：創新與新業務（New Business、Flagship DX7/DX8、ISD-ADAS、Titan table for T9X）。
  * **BG Level Focus**：短期財報獲利（讓 Anita/caymus 財務數字好看）。
* **Support 防衛策略（「但書機制」）**：
  * 面對 Corp/ISD 抽人支援的要求，建立**「防護但書」（Provisional Clauses）**。明確標註支援期限、里程碑撤離點與對原 Project 的 Impact Analysis，防止人力淪為無底洞（填坑）。

### Phase 3: WPO Precision Engine & GM Governance (WPO 精算與 GM 決策)
* **多維度精算**：整合 27P+、6G、new SOC (DX) 等 Htag，並精算 ADAS、DX7、T1000、ASIC 等 Support 人力。
* **GM Meeting 挪格與資源調配**：
  * 將 WPO 模擬結果提報至 GM Meeting。
  * **FU Focus 劃分**：確保 GM 裁示後，各 FU（WCT, WCS, WSP, WPE）的資源能精準聚焦點（例如 WCT 押寶 OO project，WCS Focus 在 XX project feature）。

### Phase 4: Closed-Loop Execution & MTime Governance (實施閉環與 MTime 運營)
* **Plan vs. Actual 轉換與監控**：
  * **Plan**：y26Q1-y26Q4 透過 **MTime** 精算專案+管理平攤。
  * **Actual**：RAS Q1-Q4 實際工時與資源反映。
* **MTime Dashboard & AI 協作**：
  * 打造 MTime Dashboard URD，即時觀察同仁投入偏離度，將偏離拉回勾選重點。
  * 將 MTime 自動轉換對齊至 RAS 與 PMO Schedule。

---

## 2. 核心營運與談判價值 (Operational Value)

```
                       ┌──────────────────────────┐
                       │    高層信任 (Trust)      │
                       └─────────────▲────────────┘
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         │                                                       │
┌──────────────────────────┐                       ┌──────────────────────────┐
│   戰略防護與但書談判      │                       │    Plan vs. Actual 實證   │
│ (Defensive Negotiation)  │                       │   (MTime / RAS Closed-Loop)│
└──────────────────────────┘                       └──────────────────────────┘
```

1. **從「被動接受抽人」轉為「帶但書的戰略對弈」**
   * **價值**：解決了組織中最痛苦的「Corp level 強制抽人 support，導致原本 BG 專案 Delay」的死穴。
   * **效益**：透過此框架，能清楚提出 Impact Analysis，讓 GM 與 Corp Level 在要求抽人時，必須同時承擔對原本 Project 進展影響的風險決策，建立健康的談判邊界。

2. **化解 Corp Level（新業務）與 BG Level（財務數字）的戰略衝突**
   * **價值**：提供一套能同時兼容 Flagship/New Business 與傳統 BU 財報數字的動態 Allocator。
   * **效益**：讓 GM 能在 GM meeting 中進行高效的「挪格（Resource Re-allocation）」，精準定調各 FU（WCT/WCS/WSP/WPE）的戰略重點，減少內部無效摩擦。

3. **MTime 至 RAS 的閉環治理（Plan vs. Actual 可追溯性）**
   * **價值**：不再讓 AOP 停留在「年初算爽的數字」，而是透過 MTime Dashboard 進行每季、每月的動態監控。
   * **效益**：即時發現資源偏離並拉回勾選事項，建立 MTime 到 RAS 的自動化轉換邏輯，為 Clare / Vincent 呈現完全可視化、可追溯且具備 AI 協作能力的終極 AOP 管理體系。

---

## 3. Claude 系統提示詞範本 (System Prompt for AI Feeding)

```markdown
# Role & Operational Context: HC AOP & WPO Expert

## 1. 核心專案背景
- 專案名稱：08/13 HC AOP (Headcount Annual Operating Plan) 準備框架
- 核心目標：建立 Top-down (財務/營運 constraints) 與 Bottom-up (專案與技術需求) 的動態平衡模型。
- 終極願景：達成「流程可視化帶來的信任感」（簡報/溝通對象：Clare / Vincent），並建立可追溯、可稽核的 AI 協作 AOP 體系。

## 2. 業務領域字典 (Domain Knowledge & Acronyms)
- **EO / FIN**: Executive Office / Finance 財務與營運管理單位。
- **WPO**: Workplan & Organization / Workload Allocation 人力與工作量動態精算機制。
- **SSA**: System & Software Architecture 系統與軟體架構時程。
- **RAS**: Resource Allocation System 既有資源分配系統（以 Q2 Baseline 為基準）。
- **MTime**: Module Time / Task Estimation 模組化工時與人力數據庫。
- **Htag**: Headcount Tagging 職等與人力標籤平衡機制（如 27P+, 6G, new SOC/DX）。
- **Key Projects & BU**: ADAS, Backend Support (DX7, T1000), ASIC, Gen99R, BG-H3 Common Part.
- **Key Executives & Stakeholders**: Rick, Joe, Clare, Vincent, Anita, caymus, GM, PM, PL, FU Heads.

## 3. 人力精算與推演邏輯 (Engine Logic)
1. **Delta 邏輯**：Project List 人力需求 = RAS Q2 Baseline + $\Delta$ (新增/異動需求，包含 3GPP R16、Modem/BU Roadmap 與 Mgr inputs)。
2. **Support Pool 扣抵與防護**：總 Target 需分攤並扣除通用組件 (BG-H3) 與支持團隊 (ADAS, DX7, T1000, ASIC)，且抽人支援必須附帶「撤離但書」與 Impact Analysis。
3. **動態平衡三要素**：
   - Total Headcount Target (from BU Top-down Target & Amort ratio)
   - Htag Balance (高階/新技術人力比例)
   - BU Allocation Ratio (各 BU 攤銷與分攤模擬)
4. **GM Meeting 挪格機制**：於 GM Meeting 決議資源重調（Resource Re-allocation），明確劃分各 FU（WCT, WCS, WSP, WPE）的核心目標專案。
5. **MTime to RAS 閉環**：Plan（MTime Q1-Q4）對比 Actual（RAS Q1-Q4），透過 MTime Dashboard 進行即時監控與偏離矯正。
```
