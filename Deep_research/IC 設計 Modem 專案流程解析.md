# **系統性解析通訊晶片（Modem IC）設計產業：從基礎架構、專案生命週期到跨部門協作**

通訊晶片（Modem IC）的設計與開發，被公認為全球半導體產業中複雜度最高、技術門檻最嚴苛的領域之一。有別於一般僅處理數位邏輯運算的處理器，Modem 晶片必須在極端受限的功耗與散熱條件下，以微秒（Microsecond）甚至奈秒（Nanosecond）為單位的即時性（Real-time），處理龐大的射頻訊號、複雜的數學編解碼，以及層層疊疊的通訊協定。這不僅需要深厚的通訊理論基礎，更仰賴一個高度結構化的專案管理體系，讓數百位甚至數千位跨領域工程師能夠精準協作。

本研究報告旨在提供一份窮盡細節、極具深度的系統性學習指南。我們將從通訊晶片的底層架構與關鍵技術規格出發，剖析其應用場景與終端產品矩陣，接著深入探討確保晶片品質的可靠度與驗證工程。最終，報告將以專案生命週期（Project Lifecycle）為主軸，展開一份詳盡的跨部門協作藍圖，從硬體（Hardware）、軟體（Software）、韌體（Firmware）、客戶支援（Customer Support）、營運管理（Operation Management）到專案管理（Program Management）的視角，細緻說明從概念發想到最終量產的每一個專案里程碑（Milestone）究竟涉及哪些具體工作與互動機制。

## **第一部分：通訊晶片核心架構與次世代技術規格解析**

在探討專案流程之前，必須先建立對 Modem 晶片底層邏輯與技術規格的正確認知。這些技術縮寫與術語，構成了晶片設計團隊日常溝通與產品定義的共通基礎。

### **晶片系統架構與運算核心配置**

在行動通訊設備的硬體架構中，應用處理器與基頻數據機扮演著截然不同卻又必須緊密耦合的角色。應用處理器（Application Processor, AP）的核心任務是執行開放式的作業系統（如 Android 或 Linux）、處理複雜的使用者介面，以及運行各種高階應用程式。其設計哲學在於提供強大的通用型運算能力與極其豐富的外設介面。相對地，基頻數據機（Modem）則是一個高度特化的即時嵌入式系統，專職處理第一層（L1）到第三層（L3）的無線通訊協定。如果將 AP 比喻為負責廣泛思考與決策的大腦，那麼 Modem 就是設備的耳朵與嘴巴，負責在充滿雜訊的環境中精準且不間斷地接收與發送訊息。

在系統整合層面，業界主要採用兩種截然不同的產品型態。第一種是獨立型基頻平台（Thin Modem Platform 或 Thin MD）。在這種架構下，Modem 是一顆實體獨立的晶片組，與 AP 物理分離。這種設計常見於資料卡（Datacard）、家用寬頻路由器（CPE），或是對 AP 效能要求極高、且需要搭配最頂尖獨立通訊晶片的旗艦級智慧型手機中。物理分離的優勢在於散熱與電源管理的獨立性，同時允許終端設備製造商自由搭配不同廠牌的 AP 與 Modem。第二種則是基頻矽智財核心（Modem Core IP）。在這種架構中，Modem 以矽智財（IP）的形式，與中央處理器、圖形處理器（GPU）及神經網路處理器（NPU）等模組共同被整合（Integration）進一顆單一的系統級晶片（SoC）之中。這種高度整合的架構能大幅縮減印刷電路板（PCB）的佔用面積，並透過共享記憶體與單一電源管理晶片顯著降低整體功耗，是目前主流中高階智慧型手機的絕對首選。

在 Modem 內部的運算核心配置上，主要依賴兩種不同特性的處理器來分擔通訊任務。數位訊號處理器（Digital Signal Processor, DSP）專職於實體層（PHY）的重度數學運算。通訊訊號的解調變需要極大量的快速傅立葉轉換（FFT/IFFT）、通道估測矩陣運算以及錯誤更正解碼。DSP 的硬體架構特別針對乘積累加運算（MAC）進行了單週期執行的最佳化，使其能在極低的時脈下完成龐大的平行運算。另一方面，微控制器（Microcontroller Unit, MCU）則負責處理 L2 與 L3 的通訊協定疊（如 RRC、MAC、RLC 層）以及系統狀態機的控制邏輯。這類 MCU（如 ARM Cortex-R 系列）具備極低的中斷延遲（Interrupt Latency）特性，確保通訊協定的交握與資源分配能符合國際通訊標準的嚴格時序要求。

### **射頻與基頻通訊規格的深層物理意義**

通訊晶片的市場競爭力，往往取決於其支援的射頻（RF）與基頻技術規格。以「4CC 300M」為例，4CC 代表四載波聚合（Carrier Aggregation with 4 Component Carriers）。這項技術允許 Modem 打破單一頻段的物理頻寬限制，同時連接並聚合四個不同的無線電頻段，將原本零碎的頻譜資源整合成一條虛擬的寬頻高速公路。而 300M 則標示了在特定配置下，該晶片或模組所能達到的最大資料傳輸速率為 300 Mbps 1。

在提升頻譜效率方面，1024QAM（正交振幅調變）是一項指標性技術。調變技術的本質是透過改變電磁波的振幅與相位來承載數位資訊。1024QAM 意味著星狀圖（Constellation Diagram）上有 1024 個訊號點，每一個通訊符號（Symbol）可以同時攜帶 10 個位元（Bits）的資料。相較於前一代的 256QAM，它能提升約 25% 的資料吞吐量。然而，密集的訊號點意味著容錯率極低。根據通訊理論，高階調變機制通常需要超過 37 dB 的極高訊號雜訊比（SNR）才能成功解碼，這表示 1024QAM 實際上只能在設備距離基地台極近、且射頻環境極度乾淨（無嚴重多徑干擾）的理想狀態下才能被觸發 4。

為了克服現實環境中的訊號衰減與干擾，天線配置技術至關重要。規格表上的 RX 與 TX 分別代表接收端（Receive）與發射端（Transmit）。其中，3RX 代表 Modem 配備了三組獨立的接收天線與射頻路徑 5。這種三接收端的設計引入了強大的空間分集（Spatial Diversity）效益。在真實的城市環境中，電磁波會因建築物反射而產生嚴重的衰落現象，甚至因使用者手部遮擋產生通訊死角。3RX 架構能從三個不同的空間維度捕捉微弱的訊號，並在基頻處理器中進行最大比例合併（Maximal Ratio Combining）。研究指出，這種空間分集架構在最惡劣的盲區環境中，能有效補償高達 13 dB 的路徑損耗（Path Loss），並顯著提升訊號干擾比（SIR），對於維持連續不中斷的網路連接具有決定性作用 6。

在多天線技術的最高殿堂，MIMO（多輸入多輸出技術）徹底改變了無線通訊的遊戲規則。MIMO 技術利用發射端與接收端的多根天線，在不增加額外頻寬的情況下，透過複雜的訊號處理技術，在同一個無線通道內創造出多重空間資料流（Spatial Streams）4。其中，多使用者 MIMO（MU-MIMO）更進一步允許基地台或路由器將頻寬精細切割，利用天線相位的偏移控制，將不同的資料流同時精準地投射給多個不同的終端設備。接收端的 Modem 則必須具備精密的通道矩陣反演演算法，利用自身多根天線的相位差，將原本混疊在一起的訊號數學重建為原始資料 4。

### **邁向非地面網路與全球物聯網**

隨著 3GPP Release 17 規範的底定，通訊晶片的戰場已從地面延伸至太空。非地面網路（Non-Terrestrial Networks, NTN）技術旨在透過低軌道衛星（LEO）、高空平台或無人機，填補傳統地面基地台無法覆蓋的廣大海洋、高山與偏遠地區，實現真正的全球無縫連網 11。NTN 技術在發展上分化為兩大分支。第一分支是 IOT NTN（或稱 NB-IoT NTN），其設計核心沿襲了地面窄頻物聯網（NB-IoT）的標準，極度強調低功耗與低成本。由於 LEO 衛星移動速度快且距離遙遠，IOT NTN 必須克服巨大的都卜勒頻移（Doppler Shift）與傳輸延遲。為了適應極其有限的傳輸資源，IOT NTN 揚棄了臃腫的 IP 協定，傾向採用非 IP 資料傳遞（Non-IP Data Delivery, NIDD）或極簡的 UDP/IP 協定，專門用於傳輸尺寸極小、頻率極低的零星訊息，例如偏遠地區的農業感測數據或緊急求救短訊 11。

第二分支則是 NR NTN（基於 5G 新空中介面的非地面網路）。相較於 IOT NTN 的窄頻特性，NR NTN 支援更寬的頻寬與更高的資料傳輸率，具備支援語音通話、視訊串流與寬頻上網的能力。然而，其技術複雜度也呈指數級上升，需要更強大的基頻運算能力來處理快速變化的衛星通道特性。NR NTN 主要瞄準高階智慧型手機、車載終端（CPE）以及具備充足電力與較大天線尺寸的自駕車系統，作為地面 5G 網路的無縫延伸 11。

在物聯網的全球部署實務中，除了強大的 Modem 晶片，還需要靈活的網路存取機制。Super SIM 便是為了解決全球跨網漫遊痛點而誕生的先進技術。由 Twilio 等平台商提供的 Super SIM 架構，採用了多重國際行動用戶辨識碼（Multi-IMSI）技術。傳統 SIM 卡被鎖定於單一電信商，而 Super SIM 允許物聯網設備在全球各地移動時，動態地從雲端下載並切換至當地最佳或最經濟的電信網路設定擋（Profiles）。更重要的是，它提供了精細的網路存取設定檔（Network Access Profiles）控制，並允許開發者透過 API 介面，直接以機器對機器（M2M）的簡訊或 HTTP 指令來喚醒、管理或重新配置遠端的 Modem 設備，大幅降低了全球物聯網營運的基礎設施成本與維護難度 16。

## **第二部分：工程品質、測試整合與除錯流暢度分析**

開發一顆包含數億個電晶體與數千萬行程式碼的 Modem 晶片，不可能不產生缺陷。因此，如何有效地發現、分類並修復這些缺陷，構成了 IC 設計日常營運的防線。

### **Triage、Validation 與 Integration 的工程哲學**

在系統單晶片（SoC）的開發過程中，Integration（整合）與 Validation（驗證）是相輔相成的兩個核心動作。Integration 是指將各個由不同團隊獨立開發的矽智財區塊（IP Blocks，如 DSP 核心、硬體加解密引擎、記憶體控制器）以及軟體模組拼接在一起的過程。這涉及解決極其複雜的跨時脈域交握（Clock Domain Crossing）與電源域隔離問題。當系統初步整合後，緊接著便需要進行 Validation。驗證的目的在於確認這個「整合後的龐然大物」是否確實符合產品規格書（PRD）的要求，並且在各種極端的使用條件（Corner Cases，如高溫低電壓、射頻訊號極度微弱）下都能保持穩定運作 23。

伴隨驗證而來的是海量的錯誤日誌（Logs）。在具備自動化測試環境的晶片公司中，每天晚上可能會有數以萬計的測試案例在伺服器農場上執行，並產生數百個失敗報告。這時便需要啟動 Triage（缺陷分流與初步診斷）流程。Triage 的原意是醫療急診室的檢傷分類，在 IC 設計中，它指的是對龐大錯誤日誌進行初步分析的過程。工程師必須在數以萬計的代碼中尋找蛛絲馬跡，判斷這個當機是因為硬體設計瑕疵、韌體狀態機陷入死鎖、還是軟體的記憶體指標錯誤。研究指出，在現代 SoC 開發中，高達 30% 的晶片設計週期被消耗在頂層的除錯與晚期缺陷的修復上。由於除錯工程師容易產生「日誌疲勞」，業界正積極導入基於機器學習（Machine Learning）的自動化 Triage 系統，用以過濾已知錯誤、預測缺陷的嚴重性，並將其準確指派給對應的負責團隊。值得注意的是，即使是自動化工具產生的「偽陽性（False Positives）」警報也具有極大價值，因為它們往往能揭露硬體與韌體邊界上隱藏的錯誤假設 25。

### **ECO、CR、Field Trial 與 SQC 實務**

當晶片完成設計並送交製造（Tape-out）後，若在矽後驗證階段發現了致命的硬體邏輯錯誤，重新製作一整套幾十層的光罩（Masks）將耗費數百萬美元與幾個月的時間。為了解決這個困境，工程師會採用工程變更命令（Engineering Change Order, ECO）。在初始晶片設計時，工程師會在各個區塊預留一些未使用的備用邏輯閘（Spare Cells）。當需要進行 ECO 時，工程師只修改最後幾層負責金屬連線（Metal Layers）的光罩，透過切斷原本錯誤的連線並重新繞線至備用邏輯閘，以極低的成本與時間代價修復硬體缺陷。這就如同不破壞建築物的水泥主結構，僅透過重新配置天花板上的管線來解決漏水問題。

與內部發現錯誤不同，客戶需求變更（Customer Request 或 Change Request, CR）通常來自於外部的市場壓力。例如，客戶在開發終端產品時，突然要求晶片必須支援某個特定的新頻段組合。CR 的處理需要專案經理與架構師進行嚴謹的影響評估，以決定是否要在當前版本實作，或是推遲到下一代產品。

實驗室裡的儀器再精良，也無法完美模擬真實世界中電信網路的混亂狀態。因此，場域路測（Field Trial）是 Modem 晶片量產前不可或缺的環節。工程團隊必須帶著搭載新晶片的測試設備，前往全球各地進行實地測試，驗證晶片在高速移動的高鐵上、訊號頻繁切換的都會區邊緣，或是面對不同設備製造商（如 Ericsson, Nokia, Huawei）基地台的特異行為時，是否能維持連線的穩定度。許多在實驗室從未出現的通訊協定死鎖，都是在 Field Trial 中被捕捉並透過韌體更新修復的 29。

當晶片正式進入量產階段，統計品質管制（Statistical Quality Control, SQC）成為營運管理團隊把關出貨品質的神經中樞。SQC 利用嚴謹的統計方法持續監控半導體製程與測試數據的變異。透過管制圖（Control Charts）、柏拉圖（Pareto Charts）與散佈圖（Scatter Diagrams），工程師可以即時發現製程良率（Yield）是否發生了異常的偏移。有效的 SQC 分析不僅能提前攔截潛在的瑕疵品，防止其流入客戶手中，更能反向辨識出測試流程中的非增值步驟與瓶頸，從而優化整體生產效率並降低成本 30。

## **第三部分：終端產品矩陣分類（Landing Productlines）**

Modem 晶片的應用早已不侷限於智慧型手機。根據對於資料吞吐量、功耗限制、移動速度、實體尺寸以及成本的不同要求，晶片設計公司通常會將研發資源系統性地分配至不同的產品線。為了清晰呈現這些產品的定位，我們將使用者查詢中的 14 條終端產品線（Landing Productlines）歸納為四大應用生態系：

| 應用生態系與特性 | 涵蓋之產品線 (Productlines) | 核心需求與 Modem 架構特徵 |
| :---- | :---- | :---- |
| **個人行動與運算平台** 極度要求功耗控制、空間微縮與峰值效能的平衡。 | Smartphone, Thin Modem, Tablet, Chrome book | 這是最尖端技術的試煉場。通常搭載最高階的 5G NR 技術，支援如 4CC 載波聚合與 1024QAM 等極限規格。在架構上，主流 Smartphone 與 Tablet 傾向使用高度整合的 SoC 以節省空間與電池；而追求極致網路效能的設備或特定的 Chrome book 則可能採用獨立的 Thin Modem 平台，透過 PCIe 介面與主系統溝通。 |
| **固網替代與行動寬頻** 無電池焦慮或配備大電池，專注於極致的資料吞吐量與多設備分享。 | MiFi, CPE, Datacard | MiFi（行動熱點）與 CPE（客戶終端設備，即家用 5G 路由器）不需考慮放入口袋的散熱限制，因此通常會全時開啟 4RX/8RX 等最高規格的多天線接收能力，並最大化 MIMO 的空間流數量，追求媲美光纖的下載速度。Datacard 則是以標準化模組（如 M.2）形式，提供給工業電腦或高階筆電擴充連網能力。 |
| **智慧車載與自駕系統** 面臨嚴苛的溫度與震動考驗，對連線的「低延遲」與「超高可靠度」有絕對要求。 | Telematics, IVT, IVI, Cockpit, ADAS | 車載環境的 Modem 設計需要符合車規（AEC-Q100）標準。Telematics（車載資通訊）與 IVT（In-Vehicle Terminal）負責車輛的基本對外連線；IVI（車載資訊娛樂）與 Cockpit（智慧座艙）需要極大頻寬以支援多個 4K 螢幕串流；而 ADAS（先進駕駛輔助）則牽涉到生命安全，其 Modem 必須支援超可靠低延遲通訊（URLLC）與 C-V2X 技術，容錯率為零。 |
| **工業物聯網與穿戴設備** 追求極低的成本、極小的尺寸以及長達數年的電池續航力。 | Redcap, IOT | 這些領域主要依賴 LPWAN 技術。IOT 設備多採用 NB-IoT 或最新的 IOT NTN（衛星物聯網）架構，犧牲傳輸速度以換取極限省電。Redcap (Reduced Capability) 則是 5G 時代的新折衷方案，它削減了標準 5G 的部分頻寬與天線數量，專門針對智慧手錶、工業監控攝影機等需要中等傳輸率但對成本與功耗敏感的設備。 |

## **第四部分：RAS 團隊職能與矽前/矽後驗證技術**

在專案從概念走向實體的過程中，RAS（Reliability, Availability, Serviceability \- 可靠度、可用性與可維護性）工程團隊是確保晶片能順利存活的關鍵戰力。他們的任務貫穿了晶片下線前後（Pre-silicon & Post-silicon）的各個階段。

在矽前（Pre-silicon）階段，**Cosim（Co-simulation，軟硬體協同模擬）** 是最核心的驗證手段。傳統的開發流程是硬體設計完成後，軟體團隊才能開始撰寫驅動程式，這種序列式的流程極易導致開發時程延宕。Cosim 技術引入了電子系統層級（Electronic System Level, ESL）的設計概念。透過在超級電腦上建立硬體電路的虛擬執行模型，軟體與韌體工程師可以在真實晶片製造出來的數個月前，就開始在虛擬平台上進行開發與除錯。這種「測試左移（Shift-left testing）」的策略，允許團隊及早發現硬體暫存器定義的錯誤，並減少系統過度設計（Overdesign）或設計不足（Underdesign）的風險，業界經驗顯示最高可節省長達六個月的開發時間 23。

與此同時，**Chip Integration（晶片層級整合）** 團隊負責確保所有數位邏輯、類比前端與射頻電路能在物理設計層面完美契合；而 **DVT（Design Verification Test，設計驗證測試）** 則是在模擬環境中，針對電壓、溫度、時脈頻率的極限邊界（Corner）進行壓力測試，確保設計具備足夠的容錯餘裕（Margin）。

當第一批矽晶片從晶圓廠送達時，專案進入高壓的矽後（Post-silicon）階段。首要任務是 **Bring up（點亮測試）**。工程師必須小心翼翼地將晶片固定在測試載板上，依序施加各組電源、輸入基準時脈，並嘗試透過除錯介面喚醒處理器，執行第一行初始化程式碼。這是一段充滿未知的過程，任何一個短路或時序錯誤都可能導致晶片毫無反應。

一旦晶片成功「點亮」，**HQA（Hardware Quality Assurance，硬體品質保證）** 團隊便會接手，依照 JEDEC 等國際標準，將晶片送入烤箱進行高溫老化測試（HTOL）、溫濕度循環測試，並進行靜電放電（ESD）的破壞性衝擊，確保晶片具備長達十年的物理壽命。

在系統整合測試方面，**SLT（System Level Test，系統級測試）** 填補了傳統自動化測試設備（ATE）的盲區。ATE 通常只以毫秒為單位測試單一接腳的電氣特性；而 SLT 則是將晶片置於真實的主機板上，載入完整的作業系統，進行長達數分鐘的高負載實際場景演練（如同時執行 4K 影片解碼與高速網路下載），藉此捕捉那些只有在長時間動態電壓波動下才會浮現的邊際效應缺陷（Marginalities）。最後，對於採用 SoC 架構的產品，還需要進行 **SOC AP Platform Integration（系統平台整合）**，確保 Modem 子系統與 AP 核心在共享記憶體、底層匯流排（如 PCIe 或 AXI）的資料搬移以及中斷控制機制上能無縫協作，達成整體系統效能的最佳化。

## **第五部分：專案生命週期（Milestone）與跨部門協作深度解析**

要系統性地學習 IC 設計產業的運作架構，最直觀且深入的方式，便是沿著時間軸，審視一顆晶片從無到有的標準里程碑（Milestones）。在以下詳盡的分析中，我們將專案生命週期劃分為三大階段，並從硬體（HW）、軟體（SW）、韌體（FW）、客戶支援（CS）、營運管理（OM）與專案管理（PM）這六大職能的視角，剖析他們在各個階段的具體任務與協作機制。

### **第一階段：產品定義與技術評估期 (Planning & Definition)**

這個階段的目標是釐清市場需求，評估技術可行性，並取得公司高層的資源授權。此時尚未開始撰寫任何一行實質的程式碼。

* **1\. MRD (Market Requirement Document, 市場需求文件)**  
  * **PM / 產品規劃團隊**：專案的起點。專案經理或產品行銷經理負責主筆 MRD，分析競爭對手的動態與電信商未來的佈建計畫（例如判斷市場是否準備好迎接 NR NTN 衛星通訊）。文件將定義晶片的目標市場、預期成本、以及必須具備的關鍵規格（如 4CC 300M、3RX 天線架構）。  
  * **CS (客戶支援)**：此時 CS 團隊必須在前線向戰略合作客戶（如頂尖手機品牌）蒐集反饋，確認 MRD 中規劃的功能確實切中客戶下一代產品的痛點。  
* **2\. TFE (Technology Feasibility Evaluation, 技術可行性評估)**  
  * **HW / FW 系統架構師**：接到 MRD 後，架構師必須評估現有的技術矽智財是否足以支撐這些規格。例如，若要支援 1024QAM，DSP 的乘法器數量需要增加多少？這將如何衝擊晶片的總面積（Area）與功耗（Power）？硬體與韌體主管在此階段會進行激烈的 HW/SW Co-design 劃分討論，決定哪些複雜的通訊演算法要燒錄成硬體邏輯（省電但無法修改），哪些要交由韌體用軟體執行（耗電但具備更新彈性）31。  
  * **OM (營運管理)**：根據架構師粗估的晶片面積，評估目前的晶圓代工廠（如台積電或三星）是否具備合適的製程節點（Node）與產能，並初步估算晶圓的製造成本。  
* **3\. PC0 (Product Council 0, 產品委員會初審) & 4\. PC1 (產品委員會複審)**  
  * **PM**：將 MRD 與 TFE 轉化為具體的商業提案，在 PC0 與高階主管進行「Go/No-Go」的決策審查。若獲准通過，專案進入 PC1 階段，此時會產出正式的產品規格書（PRD），凍結重大的規格變更，並確立嚴謹的專案時程表與人力預算配置。

### **第二階段：執行設計與實體下線期 (Execution & Tape-out)**

這是整個專案中耗時最長、壓力最大的工程實作階段。數百名工程師必須在極度緊湊的時程內完成所有的設計與驗證，最終將設計圖交付晶圓廠。

* **5\. KO (Kick off, 專案正式啟動)**  
  * **HW**：硬體工程師全面啟動，開始運用硬體描述語言（如 Verilog/SystemVerilog）撰寫數位邏輯電路（RTL）。  
  * **SW / FW**：在硬體尚未定案時，軟韌體團隊已開始在虛擬的 ESL 模擬環境（Cosim）上開發作業系統驅動程式與底層通訊協定疊，與硬體團隊並行作戰，實現測試左移 23。  
* **6\. TDI (Trial Data In, 試作資料導入) & 7\. FDI (Final Data In, 最終資料導入)**  
  * *背景說明*：TDI 與 FDI 是數位邏輯設計（前端）交棒給實體佈局繞線（後端與晶圓廠）的關鍵里程碑。  
  * **HW / 實體設計團隊**：在 TDI 階段，硬體團隊會釋出第一版初步收斂的網表（Netlist）。實體設計工程師將這些邏輯閘轉換為實際的物理佈局（Floorplanning），這時經常會發現特定區域的電線過度擁擠（Congestion）或訊號傳遞來不及（Timing Violation）。硬體工程師必須根據這些物理反饋，回去修改 RTL 程式碼。這是一個不斷往復的迭代過程。當到達 FDI 階段時，代表所有的邏輯修改都已終止，實體繞線已達近乎完美的狀態。  
  * **PM**：在這段被工程師戲稱為「死亡行軍」的期間，PM 必須每天召開站立會議，追蹤未收斂的時序違規數量，確保團隊能趕上晶圓廠預定的光罩生產檔期。  
* **8\. SO (Sign-off / Spec out, 最終簽核)**  
  * **全員**：這是一個極度嚴格的法律與工程檢核點。硬體設計必須通過設計規則檢查（DRC）與佈局一致性檢查（LVS）。所有的模組主管必須在系統上簽字畫押，擔保設計符合規格且無重大瑕疵。一旦通過 SO，晶片的命運便已註定。  
* **9\. TO (Tape out, 下線) & 10\. PG (Pattern Generation, 圖樣生成)**  
  * **OM**：當龐大的 GDSII 設計檔案被安全傳輸至晶圓代工廠後，專案宣告 Tape out。隨後進入 PG 階段，晶圓廠利用電子束將數位佈局轉換為光刻機所需的昂貴光罩（Masks）。此階段開始大量消耗實體製造資金，任何這之後發現的錯誤都只能留待日後透過 ECO 解決。

### **第三階段：矽後驗證、客戶支援與大規模量產期 (Post-Silicon & Mass Production)**

晶片變成實體後，戰場轉移到實驗室與真實世界的電信網路中。團隊必須以最快速度除錯，並協助客戶將晶片整合進終端產品。

* **11\. SB (Sample Back, 晶片回片與點亮)**  
  * **HW**：拿著示波器與三用電表在實驗室量測電壓軌與時脈訊號，確認實體晶片的基本生命跡象。  
  * **FW**：最關鍵的先鋒部隊。透過 JTAG 介面載入最底層的啟動程式碼，嘗試設定射頻暫存器，讓晶片發射出第一筆無線電波。  
  * **SW**：等待硬體與韌體穩定後，嘗試在處理器上載入 Linux 等作業系統，並建立與 AP 的通訊。  
* **12\. IMP (Internal Mass Production, 內部試量產)**  
  * **HW / FW / SW**：晶片點亮後，公司會先進行內部小批量生產，以供大規模測試。這時自動化測試會產生海量的 Bug。Triage 系統全面啟動，軟體工程師通常是檢傷分類的第一線，他們必須透過分析當機日誌，判斷是驅動程式的記憶體洩漏，還是底層韌體的狀態機死鎖 27。同時，工程團隊會帶著這批晶片進入全球 Field Trial，測試其在真實基地台環境下的極端表現 29。  
* **13\. ES (Engineering Sample, 工程樣品交付)**  
  * **CS**：將初步穩定的 ES 晶片連同參考設計板（Reference Board）與軟體開發套件（SDK）交付給早期合作的關鍵客戶（Alpha Customers）。CS 工程師會進駐客戶的實驗室，協助排除晶片與客戶電路板之間的電磁干擾或驅動不相容問題。  
  * **PM**：面對客戶在測試 ES 時提出的海量 CR（需求變更）與 Bug 回報，PM 必須主持跨部門的 Triage 會議，無情地判斷哪些缺陷是「必須在量產前修復（Must Fix）」，哪些可以透過軟體更新（Workaround）繞過，哪些則基於商業考量直接豁免（Waive）。  
* **14\. CS (Customer Sample / Commercial Sample, 商業樣品認證)**  
  * **SW / FW**：此階段晶片的硬體設計已完全凍結（若有修復皆已透過 ECO 完成），軟體與韌體也達到了可商用發佈的穩定標準。  
  * **CS**：協助客戶帶著搭載 Commercial Sample 的終端設備，前往各國的法規驗證機構（如 FCC、CE）以及嚴格的電信營運商實驗室（如 AT\&T、Verizon）進行最終的入網認證測試。  
* **15\. CMP (Customer Mass Production, 正式量產)**  
  * **OM**：客戶終端產品準備上市，晶片進入大規模生產。營運團隊全面啟動 SQC（統計品質管制）系統，監控每一批次晶圓的良率。透過調整自動化測試機台（ATE）的篩選標準（Test Limits），在保證出貨品質零妥協的前提下，最大化每片晶圓的有效產出顆粒數 23。  
  * **PM**：完成量產結案報告，將專案移交給維護團隊，並帶領核心工程師投入下一個世代通訊晶片的 MRD 規劃。

## **總結**

通訊晶片（Modem IC）的設計與落地，是一場集結了尖端物理學、精密數學演算法與龐大軟體工程的極限挑戰。從最初的市場需求文件（MRD）發想，到克服 1024QAM 與 MIMO 空間分集的物理限制；從矽前的軟硬體協同設計（Cosim），到矽後無數個日夜的 Triage 缺陷排查與 Field Trial 場域驗證，每一個專案里程碑都環環相扣。

在這條漫長且充滿風險的研發道路上，單靠卓越的電路設計或演算法創新是遠遠不夠的。唯有透過硬體、軟韌體、客戶支援與營運團隊在高度紀律的專案管理（PM）框架下進行無縫的跨部門協作，精準拿捏效能、功耗、面積與上市時間（Time-to-market）的微妙平衡，才能成功將數億個電晶體轉化為連接全球網路、推動次世代科技革命的商業基石。對於有志深耕半導體與通訊產業的專業人士而言，建立這樣一套從底層技術到專案營運的系統性宏觀視角，將是掌握產業核心競爭力的不二法門。

#### **引用的著作**

1. MT7688 WIfi OpenWRT Mini Core Board WY7688 \- ElectroDragon, 檢索日期：3月 6, 2026， [https://www.electrodragon.com/product/mt7688-wifi-openwrt-mini-core-board-wy7688/](https://www.electrodragon.com/product/mt7688-wifi-openwrt-mini-core-board-wy7688/)  
2. 4G Outdoor WIFI Router 300Mbps Waterproof 4G SIM Card Router Wide Range Wireless, 檢索日期：3月 6, 2026， [https://www.ebay.com/itm/127321602543](https://www.ebay.com/itm/127321602543)  
3. 300M Wireless WiFi Router 2.4G Access Point WIFI Long Range Signal Network Switch WAN /LAN Ports 2 Standard Plugs WiFi Repeater \- AliExpress, 檢索日期：3月 6, 2026， [https://www.aliexpress.com/i/1005008052392315.html](https://www.aliexpress.com/i/1005008052392315.html)  
4. How Does MU-MIMO Work? \- Network Computing, 檢索日期：3月 6, 2026， [https://www.networkcomputing.com/wi-fi/how-does-mu-mimo-work-](https://www.networkcomputing.com/wi-fi/how-does-mu-mimo-work-)  
5. Courier M2M 3G Cellular Modem USR3500 Reference Guide, 檢索日期：3月 6, 2026， [https://support.usr.com/support/3500/3500-files/3500-rg.pdf](https://support.usr.com/support/3500/3500-files/3500-rg.pdf)  
6. A Spatially Diverse 2TX-3RX Galvanic-Coupled Transdural Telemetry for Tether-Less Distributed Brain-Computer Interfaces | Request PDF \- ResearchGate, 檢索日期：3月 6, 2026， [https://www.researchgate.net/publication/378712012\_A\_Spatially\_Diverse\_2TX-3RX\_Galvanic-Coupled\_Transdural\_Telemetry\_for\_Tether-Less\_Distributed\_Brain-Computer\_Interfaces](https://www.researchgate.net/publication/378712012_A_Spatially_Diverse_2TX-3RX_Galvanic-Coupled_Transdural_Telemetry_for_Tether-Less_Distributed_Brain-Computer_Interfaces)  
7. US20250132878A1 \- Dynamic adjustment of adaptive reception diversity \- Google Patents, 檢索日期：3月 6, 2026， [https://patents.google.com/patent/US20250132878A1/en](https://patents.google.com/patent/US20250132878A1/en)  
8. Understanding MIMO and MU-MIMO: Enhancing Your Wi-Fi Router Performance \- AscentOptics Blog, 檢索日期：3月 6, 2026， [https://ascentoptics.com/blog/mimo-wifi/](https://ascentoptics.com/blog/mimo-wifi/)  
9. Understanding MIMO (Multiple Input, Multiple Output) \- Cellular Speed & Booster Implications \- Mobile Internet Resource Center, 檢索日期：3月 6, 2026， [https://www.rvmobileinternet.com/guides/understanding-mimo-multiple-input-multiple-output-lte-speed-cell-booster-implications/](https://www.rvmobileinternet.com/guides/understanding-mimo-multiple-input-multiple-output-lte-speed-cell-booster-implications/)  
10. \[Wireless Router\] What is MU-MIMO? | Official Support | ASUS Global, 檢索日期：3月 6, 2026， [https://www.asus.com/support/faq/1045006/](https://www.asus.com/support/faq/1045006/)  
11. Non-Terrestrial Networks (NTNs) | Anritsu America, 檢索日期：3月 6, 2026， [https://www.anritsu.com/en-us/test-measurement/solutions/non-terrestrial-networks-test](https://www.anritsu.com/en-us/test-measurement/solutions/non-terrestrial-networks-test)  
12. The Definitive Guide to Non-Terrestrial Networks \- Keysight, 檢索日期：3月 6, 2026， [https://www.keysight.com/zz/en/assets/7124-1017/ebooks/The-Definitive-Guide-to-Non-Terrestrial-Networks.pdf](https://www.keysight.com/zz/en/assets/7124-1017/ebooks/The-Definitive-Guide-to-Non-Terrestrial-Networks.pdf)  
13. Your Top Questions About NTN NB-IoT, Answered \- Ground Control, 檢索日期：3月 6, 2026， [https://www.groundcontrol.com/blog/your-top-questions-about-ntn-nb-iot-answered/](https://www.groundcontrol.com/blog/your-top-questions-about-ntn-nb-iot-answered/)  
14. What is NTN? The business case for IoT \- Onomondo.com, 檢索日期：3月 6, 2026， [https://onomondo.com/blog/ntn-iot/](https://onomondo.com/blog/ntn-iot/)  
15. NB-IoT NTN vs. 5G NR NTN: Key Differences and Use Cases \- Techplayon, 檢索日期：3月 6, 2026， [https://www.techplayon.com/nb-iot-ntn-vs-5g-nr-ntn/](https://www.techplayon.com/nb-iot-ntn-vs-5g-nr-ntn/)  
16. What is an IoT SIM Card and How is it Used in IoT devices? \- Twilio, 檢索日期：3月 6, 2026， [https://www.twilio.com/en-us/blog/what-is-an-iot-sim-card](https://www.twilio.com/en-us/blog/what-is-an-iot-sim-card)  
17. How to Measure IoT Success? | MultiTech, 檢索日期：3月 6, 2026， [https://multitech.com/wp-content/uploads/BR\_How-to-Measure-IoT-Success-1\_FINAL-1.pdf](https://multitech.com/wp-content/uploads/BR_How-to-Measure-IoT-Success-1_FINAL-1.pdf)  
18. How to build a cellular IoT device with the Raspberry Pi Pico — part one, the hardware, 檢索日期：3月 6, 2026， [https://blog.smittytone.net/2021/08/20/how-to-build-a-cellular-iot-device-with-raspberry-pi-pico-part-one/](https://blog.smittytone.net/2021/08/20/how-to-build-a-cellular-iot-device-with-raspberry-pi-pico-part-one/)  
19. Get Started with Super SIM SMS Commands and the Raspberry Pi Pico, 檢索日期：3月 6, 2026， [https://docs.korewireless.com/en-us/supersim/supersim-first-steps/get-started-with-super-sim-sms-commands-and-the-raspberry-pi-pico](https://docs.korewireless.com/en-us/supersim/supersim-first-steps/get-started-with-super-sim-sms-commands-and-the-raspberry-pi-pico)  
20. Twilio Super Sim – Public Beta | Hacker News, 檢索日期：3月 6, 2026， [https://news.ycombinator.com/item?id=23500769](https://news.ycombinator.com/item?id=23500769)  
21. LTE Cellular Ethernet WiFi IoT Edge Computer \- NCD.io, 檢索日期：3月 6, 2026， [https://ncd.io/blog/lte-cellular-ethernet-wifi-iot-edge-computer/](https://ncd.io/blog/lte-cellular-ethernet-wifi-iot-edge-computer/)  
22. 10 notable telco IoT trends—based on insights gathered in Q1 2023, 檢索日期：3月 6, 2026， [https://iot-analytics.com/top-10-telco-iot-trends/](https://iot-analytics.com/top-10-telco-iot-trends/)  
23. Overcoming Challenges in Testing at Advanced Process Nodes (5nm & Below) \- Tessolve, 檢索日期：3月 6, 2026， [https://www.tessolve.com/blogs/overcoming-challenges-in-testing-at-advanced-process-nodes/](https://www.tessolve.com/blogs/overcoming-challenges-in-testing-at-advanced-process-nodes/)  
24. Accelerating SOC Verification Using Process Automation and Integration Seonghee Yim1 Hanna Jang1 Sunchang Choi1 Seonil Brian Cho \- DVCon Proceedings, 檢索日期：3月 6, 2026， [https://dvcon-proceedings.org/wp-content/uploads/accelerating-soc-verification-using-process-automation-and-integration.pdf](https://dvcon-proceedings.org/wp-content/uploads/accelerating-soc-verification-using-process-automation-and-integration.pdf)  
25. Beyond Algorithm: Emergency Department Professionals' Perspectives on Machine Learning-Based Triage Integration—A Qualitative Study \- PMC, 檢索日期：3月 6, 2026， [https://pmc.ncbi.nlm.nih.gov/articles/PMC12535628/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12535628/)  
26. FirmReBugger: A Benchmark Framework for Monolithic Firmware Fuzzers \- arXiv, 檢索日期：3月 6, 2026， [https://arxiv.org/html/2601.15774v1](https://arxiv.org/html/2601.15774v1)  
27. Pre-Silicon Debug Automation using Transaction Tagging and Data-Mining \- DVCon Proceedings, 檢索日期：3月 6, 2026， [https://dvcon-proceedings.org/wp-content/uploads/pre-silicon-debug-automation-using-transaction-tagging-and-data-mining.pdf](https://dvcon-proceedings.org/wp-content/uploads/pre-silicon-debug-automation-using-transaction-tagging-and-data-mining.pdf)  
28. Bachelor Degree Project Bugs Prioritization in Software Engineering, 檢索日期：3月 6, 2026， [https://lnu.diva-portal.org/smash/get/diva2:1671737/FULLTEXT01.pdf](https://lnu.diva-portal.org/smash/get/diva2:1671737/FULLTEXT01.pdf)  
29. Defense Science Board, 1996 Summer Study Task Force on Tactics and Technology for 21st Century Military Superiority, Volume 3 \- DTIC, 檢索日期：3月 6, 2026， [https://apps.dtic.mil/sti/tr/pdf/ADA320452.pdf](https://apps.dtic.mil/sti/tr/pdf/ADA320452.pdf)  
30. SQC: Meaning, Examples & Analysis \- Vaia, 檢索日期：3月 6, 2026， [https://www.vaia.com/en-us/explanations/engineering/professional-engineering/sqc/](https://www.vaia.com/en-us/explanations/engineering/professional-engineering/sqc/)  
31. (PDF) Hardware/Software Co-Design \- ResearchGate, 檢索日期：3月 6, 2026， [https://www.researchgate.net/publication/2985138\_HardwareSoftware\_Co-Design](https://www.researchgate.net/publication/2985138_HardwareSoftware_Co-Design)  
32. What's in a Hardware/Software Co-design Process | Collaboration \- Learning Hub \- Altium, 檢索日期：3月 6, 2026， [https://resources.altium.com/p/whats-hardwaresoftware-co-design-process](https://resources.altium.com/p/whats-hardwaresoftware-co-design-process)  
33. Hardware/Software Codesign: The Past, the Present, and Predicting the Future \- IEEE Xplore, 檢索日期：3月 6, 2026， [http://ieeexplore.ieee.org/iel5/5/6259910/06172642.pdf](http://ieeexplore.ieee.org/iel5/5/6259910/06172642.pdf)