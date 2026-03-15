#VibeDataExpert

# 我正在為Workload系統, 進行跨季度的分攤系統開發; 目前已經到進行多個資料表的整理與串接, 需要跟你討論如何進行, 才能保有系統彈性與未來擴充性
# 主要是我想確認, 資料應該是要用**long table**或是**wide table**來整理, 會比較有系統彈性
**# 主要目的是: 我希望可以指定ras_system, ras_draft, ras_final跟amort_ratio進行後, 能夠快速地跟eo_aop, wpo_aop進行差異比較**

# 你可以提問, 先討論整理邏輯與流程
# 請用ASCII呈現流程圖

# 我目前有以下幾種資料, 主要會透過Period欄位串接, 並計算Workload的分攤結果
- 欄位說明: 
  - SurveyPeriod: 表示調查的quarter
     - 通常2026_P1: 表示是第一季的實際結果
     -  2026_P2: 表示是第二季的實際結果
     -  2026_P2_EST: 表示是第二季的預估結果
  - CurPeriod: 表示這一季的workload結果
  - NextPeriod1: 表示下一季workload結果的預估值
  - NextPeriod2: 表示下2季workload結果的預估值

1. ras_system: 屬於long table, 同一個檔案底下的SurveyPeriod, 會包含3個quarter, 3個quarter的Workload數字, 都是存在'CurPeriod'欄位, 這張表格並沒有NextPeriod1, NextPeriod2欄位

2. ras_draft: 屬於wide table, , 同一個檔案底下的SurveyPeriod, 只包含1個quarter, 3個quarter的Workload數字, 分別存在在CurPeriod, NextPeriod1, NextPeriod2欄位

3. ras_final: 屬於wide table, , 同一個檔案底下的SurveyPeriod, 只包含1個quarter, 3個quarter的Workload數字, 分別存在在CurPeriod, NextPeriod1, NextPeriod2欄位

4. eo_aop: 屬於wide table, 同一個檔案底下的SurveryPeriod, 會變成header, 表示每一個季度的workload分攤後的目標, 所以會有欄位名稱是2026_P1, 2026_P2
5. wpo_aop: 屬於wide table, 同一個檔案底下的SurveryPeriod, 會變成header, 表示每一個季度的workload分攤後的目標, 所以會有欄位名稱是2026_P1, 2026_P2
6. amort ratio: 並不會合併成單一檔案, 每一季都是獨立的檔案, 但每一季分別會有實際值與預估值兩種結果, 例如: 2026_P1_act與2026_P1_est分別表示實際值與預估值


# 更新頻率
1. ras_system: 每一季更新多次
2. ras_draft: 每一季更新多次
3. ras_final: 每一季更新1次
4. eo_aop: 一年更新一次
5. wpo_aop: 一年更新多次
6. amort ratio: 每一季更新


#amort_ratio 指的是將技術單位的專案workload分攤給事業單位的比例, 所以是將技術單位與事業單位連結起來的必要過程
#事業單位: eo_aop與wpo_aop看的都是事業單位的分攤後結果

