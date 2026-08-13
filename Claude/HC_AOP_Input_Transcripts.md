# HC AOP & WPO 手寫筆記完整整理與抄錄 (Input Photo Transcripts)

## 圖片一 & 圖片二：08/13 HC AOP 準備 Framework 與 WPO 邏輯

### EO Timeline
* 交付時間
* 交付格式
* review 流程
* EO Tech LL 調查
* FIN 安排 BU roadmap meeting
* 協助收斂 key project list
* 提供 Amort ratio
* BU Top down Target

---

### Project Roadmap
* $\rightarrow$ 外部／市場／技術, 3GPP R16 官方規劃
* $\left. \begin{array}{l} \rightarrow \text{Modem Roadmap} \\ \rightarrow \text{SSA schedule} \end{array} \right\}$ SOC 為主
* $\rightarrow$ BU roadmap $\}$ 產品為主
* $\rightarrow$ Mgr inputs (PM, PL, BU head, Coord...)
* $\rightarrow$ Project list:
  * 以 RAS Q2 為基礎，加上以上 delta
* $\rightarrow$ BU define: key project (需預估的 product project)
* Tech Project EOL

---

### WPO framework
* $\rightarrow$ WTG HC Target (factor: MCD/CTD/CD2, RSS, other BU)
  * $\rightarrow$ 年度預估等階 $\rightarrow$ 重要 factor: 27P+, 6G, new SOC (DX), AGM + meeting
* $\rightarrow$ project list (工作檔案公式，參考數據 MTime; RAS)
* $\left. \begin{array}{l} \rightarrow \text{Tech} \\ \rightarrow \text{product} \end{array} \right\}$ 更新到工作檔案，RAS vs 去年預估值現值
* $\rightarrow$ 主管預估:
  * Project, mgr, factor (需準備多考資料，excel 檔案，去年、今年比較)
* $\rightarrow$ DSOT 預估方式
* $\rightarrow$ BG-H3 common part
* $\rightarrow$ WTG support 人力:
  * ADAS
  * Backend support: DX7, T1000
  * ASIC
* $\rightarrow$ Top Down 管理人數:
  * ADAS
  * Gen99R
* $\rightarrow$ 重理去年、前年預估 logic
* $\rightarrow$ 總人數調整 $\left[ \begin{array}{l} \text{總人數} \\ \text{Htag 平衡} \\ \text{BU分攤總數} \end{array} \right. \quad \text{BU Top down Target} \right]$
* $\rightarrow$ MTime pre sum
* $\rightarrow$ RAS pre sum $\quad$ with SSA project milestone
* $\rightarrow$ BU 分攤模擬
* $\rightarrow$ BU 討論及調整

---

### 關鍵戰略目標 (左下角筆記)
* **# 流程可視化帶來的信任感 to Clare/Vincent**
* **# 建立 AOP 框架與 AI 協作，且可追蹤**
* **# 整理紙本筆記**

---

## 圖片三 & 圖片四：組織角力、支援防護與 MTime/RAS 落地機制

### 1. 組織架構與利益衝突 (Corp Level vs. BG/BU Level)
* **WTG 水位 Dashboard (AOP Next Year HC AOP)**
  * **組織分工**：Rick / Joe 下轄 RD、CT、JC、PM、ISD 等單位。
* **AOP 數字來源與角力**：
  * **期待差異**：會有來自 PM、BG/BU 的基本期待，同時也有 Corp level（Rick / Joe）想要做的 Project。
  * **資源卡控與排擠**：對 Resource 的投入有所卡控，且被排擠到的 Project 可能是 BG 想要做的。
  * **重點專案範例**：
    * Flagship (DX7 / DX8)
    * ISD-ADAS
    * ASIC
    * Project D for caymus
    * Titan table (S11, S12, 加外接式) for T9X
  * **戰略目標衝突**：Top corp level 期望 **new business**；BG level 則希望讓 Anita, caymus **財務數字好看**。

### 2. Support 抽人與防禦策略
* **面對 Corp level / ISD 要求抽人 Support 特定 Project**：
  * **WTG 回應機制**：WTG 要如何回應？要有好的**回應方式加但書**。
  * **風險控管**：避免投入太多人進去「填坑」而難以抽身，也有可能會影響到原有 Project 的進展。

### 3. GM 決策與資源挪格 (GM Meeting)
* **決策關卡**：所以 WPO HC AOP 預估值要過 **GM meeting** 確認。
* **GM 考量**：GM 會有要調整且**挪格（Resource Re-allocation）**的考量。
* **目標拆解與內部溝通**：
  * 明年 HC AOP 是訂好的目標要強 GM，帶回去劃分出各 FU 的重點。
  * **範例**：例如 WCT 重心放在 OO project 而非 XX project；WCS 重心是 XX project 的 feature 而非 OO project；WSP, WPE 通過 OO project 驗證而非 OO project（再由 FU 內部溝通）。

### 4. MTime 與 RAS 機制實務 (Plan vs. Actual)
* **Plan vs. Actual 對比機制**：
  * **Plan (y26Q1 - y26Q4)** $\rightarrow$ MTime
  * **Actual** $\rightarrow$ RAS Q1 - Q4
  * **產出** $\Rightarrow$ Project + [Mngt 平攤過去]
* **MTime 治理與落地機制**：
  * **建立 MTime 溝通框架**：定期提供 MTime data to GM / PL。
  * **動態追蹤**：觀察同仁投入動向，即時引導與拉回勾選的重要事項。
  * **系統轉換與呈現**：
    * 是否由 MTime 轉換到 RAS？
    * 是否呈現 Project Schedule $\Rightarrow$ PMO？
    * MTime Dashboard URD。
