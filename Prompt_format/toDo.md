# 1.To Do Task
1. RAS平台AI構想

## 長期目標
- 用AI做即時通報系統
- 建立資料流動平台 -> AI診斷 -> 通知 -> 跨資料sync(分析)
- 全流程數位化分析

## 想要解決的問題
- RAS分析模板
- M Time 分析模板, Dashboard版面, ETL flow(+API) 
- HC AOP AI模板
- M Time to RAS Algorithm

## Idea 發想
0. 分層次將資料給AI, 讓AI分析並提出下一個問題, 且同時提供撈資料的code(text to sql)
1. 用Data Schema問AI, 可以分析什麼問題? (需要Prompt設計), 產出各種不同的分析維度的python function
2. 用AI設計, python資料整理完後的輸出格式(打包小包資料)
3. 將小包資料加入分析prompt, 進行輕量化分析
4. 最後手動將Prompt與小包資料貼到copilot, 進行分析, 再畫圖, 並產出HTML版本的報告(one page)

```
現在的問題應該是要解決, 用現有技術做一個AI即時通報系統
# 建立資料流動平台 -> AI診斷 -> 通知 -> 跨資料分析
# Prompt
# VibeConsult

# 我會的技術
- python: 3.8
- IDE: spyder, jupyter lab
- python package: flask, streamlit, echart
- local DB: maria db, heidi sql
- Tableau
- sharepoint, wiki
- gitea
```

# 2.AI應用
## AI新知
- 做CLAUDE.md
- 做Agent skills
- Perplexity Computer

## AI使用技巧
1. 先看AI犯錯, 再問他為什麼犯錯與如何避免


# 3.Prompt Pool
```
# VibeConsult
- 我想要建立一個工作紀錄的工具, 可以讓從日常工作中, 累積執行經驗, 並往上總結成週報與成果展現, 再連接到個人年度績效 

# 功能發想:
- 我需要能夠記錄每天的to do list, 還可以標記重要性與合作窗口
- 需要能夠紀錄lesson learnt與相關的檔案及計算邏輯, 檔案可以能會email, powerpoint, excel為主
- 需要能夠讓AI或是LLM model能夠讀懂這些資訊, 並為我整理出週報, 與撰寫成果
- 需要能夠讓AI或是LLM model能夠讀懂這些資訊, 並將內容累積成營運知識庫, 可以重現處理流程與框架, 或是根據我的問題回答過去所做的事情

# 我會的技術
- python: 3.8
- IDE: spyder, jupyter lab
- python package: flask, streamlit, echart
- local DB: maria db, heidi sql
- Tableau
- sharepoint, wiki
- gitea
```

# 3-1.Prompt Pool - RAS Platform
```
#VibeCoding
我現在需要針對**RAS**開發**資料流動平台**, 並做成**通報系統**
- 資料流動平台需要包含全流程的數位化分析, AI診斷, 通知, 與跨資料源分析

# 關鍵要素
- 能夠**自動**且**快速**計算各產品線分攤後的結果
- 能夠進行drill down分析
- 能夠進行QoQ比對

# 先有基礎功能, 再拓展資料源

# 系統流程

## 1.讀取資料模組
- 這個階段需要讀取資料, 並進行必要的欄位清洗(rename column)
  1.ras_frozen
  2.ras_pie
  3.ras_tmp
  4.amort_ratio
  5.eo_aop
  6.wpo_aop
  7.project_milestone
  8.tech_wireless_modem_config

## 2.QoQ偵測模組
- 這個模組需要針對讀進來的資料進行QoQ分析比對, 屬於描述性的分析
  1.amort_ratio: (1) 各產品線比例分攤, (2) Shippment差異, (3) BG-H3: Common part, Corp H3
  2.ras_frozen: from gen level to project level
  3.ras_pie: from gen level to project level
  4.ras_temp: from gen level to project level

## 3.模組化計算流程
- 這個階段會需要定義, 需要計算的維度與資訊
- (factor_x) 格式: {source} * {amort_ratio} = {amorted_result}
- (factor_y) 格式: {eo_aop}, {wpo_aop}

## 4.預期輸出格式
- 這個模組會定義, 計算之後的產出格式, 先以excel format為主(需要呈現什麼資訊, 排列columns, rows; Gen/Project)
- 後續會把每次分析的格式都系統化先用python產出
- 中期: 再做到python畫圖(jupyter lab pgwalk), html畫圖, AI產結果

## 5.AI診斷模組
- 這個模組會定義, 各種資料源的資料, 可以進行的初步分析
  1.amort_ratio
  2.ras_frozen:
     - Org level: 實際填寫人數, Regular, DS, OT人數與分布
     - Tech level:
     - Special project: support的人數與組織是否有差異,
         - 定義Special project的project code config table



```

# 4.Finish Items
1. (03/07, Done) 先做Modem Deep Research
2. (03/07, Done) 效能助手打包
