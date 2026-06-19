# **系統單晶片設計流程與先進通訊架構深度剖析：聚焦聯發科技5G數據機技術與發展**

隨著行動通訊技術的演進與半導體製程的微縮，現代通訊基頻（Baseband）系統單晶片（System-on-a-Chip, SoC）的設計複雜度呈現指數型增長。在5G-Advanced（3GPP Release 18）及未來的6G標準推動下，通訊設備不僅需要處理多千兆位元（Multi-Gigabit）的資料吞吐量，還必須在極限功耗、散熱與晶片面積（PPA）之間取得完美的平衡。本研究深入梳理積體電路（IC）設計流程與通訊協定堆疊的底層邏輯，並以聯發科技（MediaTek）的先進5G數據機（如M90）與天璣（Dimensity）系列SoC為核心，進行全方位的技術架構與產業影響剖析。

## **第一部：系統單晶片與特殊應用積體電路之頂層設計流程**

現代通訊晶片的開發是一項高度協同的工程，涵蓋從抽象的系統規格定義到實體矽晶圓製造的完整週期。ASIC與SoC的設計流程被嚴格劃分為前端（Front-End）邏輯設計與後端（Back-End）實體設計，每一個階段皆需要專屬的電子設計自動化（EDA）工具與驗證方法學來確保最終矽智財（IP）與晶片能順利運作1。

### **系統規格定義與微架構設計**

任何ASIC專案的首要步驟是定義晶片的系統規格與架構。系統架構師會評估應用程式的需求、效能目標、功耗限制以及實體面積要求，將這些資訊轉化為高階的方塊圖。在通訊SoC中，架構師必須決定硬體與軟體的劃分（Hardware/Software Partitioning），例如決定哪些複雜的數位訊號處理（DSP）演算法（如快速傅立葉轉換或錯誤更正編碼）需要專屬的硬體加速器，哪些則交由嵌入式微處理器透過應用程式介面（API）以軟體執行2。為加速複雜設計的開發，高階合成（High-level Synthesis, HLS）技術常被採用，工程師使用C或C++等高階語言建立軟體模型，再由工具將演算法轉換為硬體描述語言8。

### **暫存器傳輸級設計與前端驗證**

在確定架構後，RTL（Register-Transfer Level）工程師會使用Verilog或SystemVerilog等硬體描述語言撰寫可合成的程式碼。RTL設計著重於資料路徑（Data path）、控制邏輯與有限狀態機（FSM）的具體行為，並廣泛利用模組化設計以提升IP的重用性4。在此階段，效能、功耗與面積（PPA）的最佳化即已展開，設計團隊會使用如Ansys PowerArtist等工具進行暫存器傳輸級的功耗估算，標記程式碼中無效的翻轉（Toggle）以減少動態功耗浪費8。  
RTL編碼完成或同步進行的關鍵步驟是功能驗證（Functional Verification）。驗證團隊通常採用通用驗證方法學（UVM）建立測試平台（Testbench），透過隨機測試與循環基礎的驗證來確保RTL碼在各種極端情境下均符合規格3。此外，工程師必須執行Linting檢查以排除語法與基本邏輯錯誤，並利用形式驗證（Formal Verification，如Jasper Gold）與斷言基礎驗證（Assertion-based Verification）來確保設計意圖與規格完全吻合10。若RTL設計包含多個時脈域，則必須使用SpyGlass或Questa CDC等工具進行時脈域交接（Clock Domain Crossing, CDC）分析，以防止亞穩態與資料遺失12。

### **邏輯合成與可測試性設計**

功能驗證達標後，設計將進入邏輯合成（Logic Synthesis）階段。合成工具（如Synopsys Design Compiler或Yosys）會將抽象的RTL碼映射到特定的技術節點標準元件庫，生成閘級網表（Gate-level Netlist）3。此轉換過程會同步進行初步的時序與面積最佳化，確保電路邏輯在目標頻率下具備可行性。  
在進入後端設計前，可測試性設計（Design for Testability, DFT）團隊會介入，於電路中插入專屬的測試邏輯，以提升製造後晶片的瑕疵檢測率並控制測試成本3。主要的DFT技術包含掃描鏈（Scan Chain）插入，將設計中的正反器串接以提高內部狀態的控制度與可觀察性；記憶體內建自我測試（MBIST），用於快速檢測SoC內龐大的SRAM區塊；以及自動測試圖樣產生（ATPG），將測試向量轉換為測試機台可讀取的WGL格式4。

### **後端實體設計與時序收斂**

後端實體設計（Physical Design）負責將閘級網表轉換為矽晶圓製造所需的實體佈局（GDSII格式），此過程對於先進奈米製程的良率與效能影響深遠3。

| 實體設計階段 | 核心任務與技術細節 |
| :---- | :---- |
| **平面規劃 (Floorplanning)** | 決定晶片面積、I/O腳位、大型巨集（Macro）與IP模組的物理位置。必須建立穩健的電源網格（Power Mesh）以均勻分佈電流，防止IR Drop（壓降）與地彈（Ground Bounce），並預留充足空間以避免後續繞線壅塞3。 |
| **擺置 (Placement)** | 將數以千萬計的標準元件放置於預定列中。分為全域擺置（Global Placement，大致分佈以縮短總線長）與詳細擺置（Detailed Placement，確保元件不重疊且符合設計規則）3。 |
| **時脈樹合成 (CTS)** | 建立低偏斜（Low Skew）且平衡的時脈分配網路。常採用H-Tree、X-Tree或Mesh等混合結構，以確保時脈訊號同步抵達所有正序元件，並優化動態功耗5。 |
| **繞線 (Routing)** | 為所有邏輯腳位建立實體金屬連線。在先進製程中，繞線必須克服嚴重的訊號完整性（Signal Integrity）問題，包括串擾（Crosstalk）與電遷移（Electromigration）2。針對天線效應（Antenna Effect），需透過跳線（Metal Jumpers）或插入天線二極體來釋放製程中累積的電荷12。 |
| **靜態時序分析 (STA)** | 使用PrimeTime等工具，結合提取的寄生參數（SPEF），全面驗證資料路徑的建立時間（Setup Time）與保持時間（Hold Time）裕度。若發現負寬裕（Negative Slack），需反覆進行佈局最佳化9。 |

### **實體驗證與流片簽核**

在最終流片（Tape-out）之前，設計必須通過嚴格的實體驗證（Physical Verification）與簽核（Sign-off）流程。此階段使用Calibre或Magic等工具執行設計規則檢查（DRC），確保金屬線寬、間距與通孔密度符合晶圓廠的製造限制；接著進行佈局對接電路圖驗證（LVS），比對實體佈局提取的網表與合成網表是否完全一致；最後執行電氣規則檢查（ERC）與靜電放電（ESD）防護驗證，確保晶片具備足夠的可靠性3。

### **晶片設計之高階挑戰：時脈域交接與晶片內網路**

在複雜的通訊SoC中，各個子系統（如CPU核心、DSP、硬體加速器與各類介面）通常運行於獨立且非同步的時脈域。當資料跨越這些邊界時，時脈域交接（CDC）問題便應運而生15。若輸入訊號在接收端正反器的時脈邊緣附近發生轉態（違反建立或保持時間），將導致亞穩態（Metastability）。亞穩態會使正反器輸出停留在未定義的電壓位準，經過一段不可預測的解析時間後才隨機穩定於高電位或低電位，進而造成資料損毀或邏輯錯誤15。  
亞穩態的平均故障間隔時間（MTBF）是一個關鍵的可靠性指標，其數學模型顯示MTBF與解析時間呈指數正相關：  
![][image1]  
為提升MTBF，設計人員不能單純依賴佈局繞線工具，必須在RTL階段引入多級同步器（Multi-stage Synchronizers）、非同步FIFO緩衝區或交握協定17。此外，傳統的重置訊號（Reset）若跨越時脈域，亦需要重置同步器以確保安全釋放；而閂鎖器（Latch）基於其透明導通的特性，在CDC路徑中更易引發難以追蹤的時序錯誤，需要特殊的靜態分析策略來推斷其虛擬時脈域18。  
另一方面，晶片內網路（Network-on-Chip, NoC）成為解決多核心與AI異質運算資料搬移瓶頸的關鍵。隨著人工智慧與神經網路處理單元（NPU）的整合，傳統匯流排無法提供穩定的頻寬與確定性的仲裁。現代NoC設計需要具備實體感知（Physically aware）的拓撲結構，區隔對延遲敏感的控制訊號與高頻寬的資料負載。同時，架構師必須謹慎劃分快取一致性（Coherency）網域，避免過度的探測（Snoop）流量消耗寶貴的繞線資源並推升功耗1。

## **第二部：5G NR 通訊協定堆疊與數據機基頻處理架構**

上述複雜的SoC設計方法論，在通訊晶片領域主要被用於實現3GPP定義的行動通訊協定堆疊。從4G LTE邁向5G新無線電（New Radio, NR），通訊架構經歷了從「承載導向（Bearer-based）」至「資料流導向（Flow-based）」的典範轉移，這深刻影響了基頻數據機的軟硬體協同設計21。  
5G NR協定堆疊被劃分為控制面（Control Plane，負責信令與連線管理）與使用者面（User Plane，負責應用程式資料傳輸）。

### **服務資料適配協定層 (SDAP) 的引入與影響**

SDAP是5G NR為了適配5G核心網（5GC）全新的QoS架構而獨創的層級。在LTE時代，每個演進封包系統（EPS）承載僅對應一種QoS設定，導致當應用程式需要多種不同優先級的傳輸時，必須建立大量的資料無線電承載（DRB），這會成倍消耗數據機的記憶體、PDCP/RLC實體資源與RRC信令開銷22。  
SDAP解決了這個擴展性問題。它負責將來自核心網的各種QoS流（QoS Flows）映射到單一或多個DRB上。當封包抵達SDAP層時，若多個QoS流共用一個DRB，SDAP會在封包前方插入一個輕量級的標頭，標記6位元的QoS流識別碼（QFI）與反射性QoS指示（RQI）22。這種設計使得5G設備能以極低的資源代價同時處理超可靠低延遲通訊（URLLC）、高頻寬影音串流與背景物聯網資料，並支援動態的網路切片（Network Slicing）22。

### **核心協定層次：PDCP、RLC 與 MAC**

資料經過SDAP映射後，依序向下穿越PDCP、RLC與MAC層，這三層在5G基頻晶片中通常由高效能的微控制器與專屬的硬體加速器共同處理。

| 協定層級 | 核心功能與基頻設計影響 |
| :---- | :---- |
| **封包資料匯聚協定 (PDCP)** | PDCP負責資料的安全性與傳輸效率。其核心功能包含ROHC（穩健標頭壓縮）以縮減IP/UDP/RTP標頭負載；控制面與使用者面的加密解密（Ciphering/Deciphering）與完整性保護（MAC-I驗證）。在5G雙連線（NR-DC）或換手過程中，PDCP利用擴展序號（COUNT）進行封包重新排序與重複偵測。5G更新增了封包複製（Duplication）功能，為URLLC應用提供極致的可靠性21。 |
| **無線電鏈路控制 (RLC)** | RLC確保無線電鏈路的可靠傳輸，提供透通模式（TM）、非確認模式（UM）與確認模式（AM）。在AM模式下，RLC獨立於PDCP維護自身的序號，並透過自動重傳請求（ARQ）進行錯誤更正。它也負責將來自上層的服務資料單元（SDU）進行切割（Segmentation）與重組，以適配MAC層排程的傳輸區塊大小22。 |
| **媒體存取控制 (MAC)** | 作為資料連結層的底層，MAC層將多個邏輯通道多工（Multiplexing）成傳輸區塊（TB），交由實體層傳送。MAC層負責執行混合式自動重傳請求（HARQ）的錯誤修正、隨機存取程序（Random Access Procedure）以及向基地台發送排程資訊回報。其排程效率直接決定了通訊晶片的延遲表現23。 |

### **實體層 (PHY) 與高層控制協定 (RRC/NAS)**

實體層（PHY）是通訊晶片中算力需求最龐大的部分。它接收來自MAC層的傳輸區塊，進行前向錯誤更正編碼與解碼（FEC Encoding/Decoding）、通道編碼的速率匹配、HARQ軟合併（Soft-combining）以及複雜的高階調變與解調23。在5G環境中，PHY層還必須處理大規模多輸入多輸出（Massive MIMO）的波束成形（Beamforming）與波束追蹤，這需要基頻晶片內建極度優化的矩陣運算硬體，且與天線射頻前端緊密協作24。  
在控制平面，無線資源控制層（RRC）主導設備與無線接取網路（NG-RAN）之間的信令交互。它負責廣播系統資訊、建立信令與資料無線電承載（SRB/DRB）、管理行動性（如換手與小區重選），並維護複雜的狀態計時器（如T300連線建立計時器、T310無線電鏈路失敗計時器）21。最頂層的非存取層（NAS）則直接與5G核心網的AMF節點對話，處理IP位址分配、身分驗證與會話管理21。

## **第三部：聯發科技 (MediaTek) 先進數據機與通訊平台解析**

聯發科技將先進的SoC實體設計能力與深厚的3GPP標準參與經驗相結合，發展出領先業界的5G數據機與天璣平台。其技術版圖涵蓋了追求極限頻寬的5G-Advanced、強調能效的物聯網通訊，以及前瞻的非地面網路佈局28。

### **5G-Advanced 與 M90 旗艦數據機架構**

MediaTek M90是業界率先對齊3GPP Release 17並兼容即將到來的Release 18（5G-Advanced）標準的旗艦數據機解決方案31。M90的架構設計突破了既有5G通訊的頻寬與效率天花板，在與合作夥伴（如Keysight、Ericsson與Verizon）的實網與實驗室測試中，展示了驚人的資料吞吐量31。

| 技術維度 | M90 數據機架構特徵與極限效能 |
| :---- | :---- |
| **最高資料傳輸率** | 下行峰值高達 12Gbps。在FR1+FR2雙連線（NR-DC）環境下實測下載速度達11.6Gbps；在FR1單獨組網（SA）的6CC載波聚合測試中達5.5Gbps31。 |
| **載波聚合 (CA) 能力** | 支援 Sub-6GHz (FR1) 最高 6CC-CA；毫米波 (FR2) 最高 10CC-CA，並支援高階256-QAM調變31。 |
| **上行增強與MIMO** | 支援 Rel-17 2T-2T 上行傳輸天線切換（Uplink TX switching），將上行效能提升20%。與三星合作完成業界首創的 3Tx 5-layer 上行配置，顛覆上行頻寬限制31。 |
| **多卡雙通與智慧天線** | 支援 5G 雙卡雙通（DSDA）與雙資料能力，確保兩張SIM卡擁有獨立射頻路徑，支援混合雙工（TDD+FDD）無縫切換。內建Smart AI Antenna技術，精準識別環境達99.5%，提升資料吞吐量最高達30%31。 |

值得注意的是，第三方實測數據顯示，聯發科技的Release 17架構（如Dimensity 9500）在擁擠環境下的上行速率（11.7 Mbps）、延遲（28 ms）與抖動表現，顯著優於搭載高通X85晶片的競品。這得益於聯發科技在Power Class 2上行鏈路與2x2上行MIMO設計上的架構優勢39。

### **人工智慧驅動的底層能效：MMAI 與 5G UltraSave 4.0**

M90及最新天璣SoC中最具顛覆性的創新是引入了**MediaTek Modem AI (MMAI 2.0)**。這代表通訊晶片設計從被動的閾值反應轉向基於AI的「主動預測」模型31。MMAI能透過裝置端的機器學習模型，即時分析網路條件、數據流量特徵（如爆發式下載與背景物聯網微量封包）以及使用者的物理狀態（如移動中、室內外環境或握持姿態）31。  
結合MMAI的主動預測，**MediaTek 5G UltraSave 4.0** 節能套件在硬體層面實現了多維度的功耗控制，將平均功耗較前代降低18%31：

1. **頻寬部分動態調整 (Dynamic BWP)：** 根據MMAI預測的流量需求，在低負載時將數據機切換至較窄的頻段，大幅降低射頻接收器的開啟時間與靜態功耗35。  
2. **連接模式非連續接收 (C-DRX) 與喚醒訊號 (WUS)：** 優化排程演算法，使數據機在資料傳輸空檔進入深度睡眠，僅在收到網路低功耗喚醒訊號時才啟動監聽35。  
3. **尋呼提早指示 (Paging Early Indication, PEI)：** 整合3GPP Rel-17的PEI標準，允許網路提前告知終端即將到來的尋呼訊息。此機制減少了不必要的控制通道盲解碼，使閒置功耗進一步降低15%35。  
4. **副載波休眠 (SCell Dormancy) 與電壓最佳化：** 在多載波聚合情境下，迅速將未使用的副載波置於休眠狀態，並動態調整數據機內部邏輯元件的供電電壓以適配當下負載35。

### **RedCap (縮減能力) 物聯網通訊平台：T300 RFSOC**

針對無需極致頻寬、但對電池壽命與體積極度敏感的穿戴式設備與工業物聯網（IIoT），聯發科技基於3GPP Release 17的**5G RedCap (Reduced Capability)** 標準，推出了M60數據機IP與T300系列平台28。  
T300系列是業界首款採用台積電6奈米製程的單一晶粒射頻系統單晶片（Single-die RFSOC），它將基頻處理器、射頻子系統與Arm Cortex-A35核心完美整合41。

| RedCap 平台優勢 | T300 / M60 架構特性 |
| :---- | :---- |
| **硬體精簡與緊湊設計** | 透過限制在單一載波（1CC, 20MHz）與1T2R（單發雙收）MIMO天線配置，大幅縮減了射頻前端與天線設計的複雜度，使核心PCB面積較傳統4G IoT方案縮小達60%30。 |
| **連線效能與規格** | 支援5G獨立組網（SA），下行峰值可達227Mbps，上行可達122Mbps。支援高階256-QAM調變、網路切片技術與雙卡單通（DSSA）41。 |
| **突破性的功耗表現** | 結合RedCap專屬的UltraSave 4.0機制，包含UE子群組化、TRS閒置資訊、PDCCH監控自適應與RLM（無線電鏈路監控）放寬等Rel-17增強功能。其功耗較4G LTE方案降低60%，較5G eMBB方案更大幅減少70-75%41。 |

### **非地面網路 (NTN) 與衛星通訊整合**

聯發科技是推動智慧型手機與物聯網裝置直接連結衛星網路的先驅。其M90數據機與車用旗艦晶片（如Dimensity Auto Connect MT2739）領先業界，將非地面網路（NTN）技術與地面蜂巢式網路整合於同一IP中28。這要求IC在設計時必須具備高度靈活的DSP引擎，以補償衛星通訊帶來的巨大都卜勒頻移（Doppler Shift）與訊號延遲。  
架構上支援兩種3GPP NTN標準檔：

1. **IoT-NTN (基於窄頻)：** 針對資產追蹤、農業監控與緊急求救等低速率（數十至數百kbps）情境。其源自NB-IoT架構，完美契合UltraSave低功耗特性，確保在無網路覆蓋區的基本遙測與通訊31。  
2. **NR-NTN (基於新無線電)：** 採用5G NR空中介面技術，專為高資料速率的寬頻服務設計。透過低軌道（LEO）衛星，能提供接近100Mbps的連線速度，使視訊通話與網頁瀏覽在偏遠地區或海上成為可能27。

## **第四部：聯發科技高階系統單晶片 (SoC) 與跨領域應用**

聯發科技的技術佈局並非僅限於獨立的數據機晶片，更透過其天璣（Dimensity）與相關系列平台，將先進通訊、人工智慧運算與多媒體處理完美封裝於頂級SoC內。

### **天璣 9500 (Dimensity 9500\) 與 9500s 旗艦行動平台**

天璣 9500 系列代表了目前行動運算領域的最高水準，採用台積電第二代3奈米（N3P）先進製程，專注於提供媲美PC等級的運算能力、主動式AI（Agentic AI）支援與極致的遊戲體驗30。  
架構上，Dimensity 9500延續並改良了「全大核（All-Big-Core）」CPU設計理念，捨棄傳統的節能核心，以達到極致的多執行緒吞吐量。它配備了1顆時脈高達4.21GHz（在9500s版本中為3.73GHz）的旗艦級Arm Cortex-X925（C1-Ultra）核心、3顆Cortex-X4效能核心與4顆Cortex-A720核心51。為餵飽這些強悍的核心，聯發科技為其設計了巨大的快取架構：19MB的L2+L3快取，外加10MB的系統層級快取（SLC），並領先業界支援4通道UFS 4.1儲存，使大型AI模型載入速度暴增40%52。  
在人工智慧方面，整合的**第9代 NPU 990 (Generative AI Engine 2.0)** 具備獨創的記憶體內運算（Compute-in-Memory, CIM）架構。CIM大幅減少了AI權重在記憶體與處理單元間來回搬移的功耗。此外，硬體原生支援BitNet 1.58-bit模型壓縮技術，降低了33%的AI功耗，並允許在裝置端流暢運行Stable Diffusion XL等生成式影像模型，與處理高達128K Token長度的文本51。

| 平台特性對比 | MediaTek Dimensity 9500 | MediaTek Dimensity 9500s | MediaTek Dimensity 9200 Plus |
| :---- | :---- | :---- | :---- |
| **製程節點與架構** | 3nm，第三代全大核設計 | 3nm，第三代全大核設計 | 4nm，傳統1+3+4混合架構 |
| **CPU 頂級核心時脈** | Cortex-X925 @ 4.21GHz | Cortex-X925 @ 3.73GHz | Cortex-X3 @ 3.35GHz |
| **GPU 與遊戲技術** | Arm Mali-G1 Ultra MC12，支援120FPS 光線追蹤，UE 5.5+ | Arm Mali-G1 Ultra MC12，支援165FPS 極限幀率 | Immortalis-G715 MC11 |
| **AI 處理單元** | NPU 990 (具備CIM與BitNet支援) | 高階 NPU (支援SDXL裝置端生成) | APU 690 |
| **數據機與通訊** | 5G-Advanced (Rel-17/18)，最高 7.4Gbps DL，支援 Wi-Fi 7 (7.3Gbps) | 5G (Rel-17)，最高 7Gbps DL，支援 Wi-Fi 7 (6.5Gbps) | 5G (Rel-16)，最高 7.9Gbps DL，Wi-Fi 7 |
| **影像處理 (ISP)** | Imagiq 1190 (4K120 Dolby Vision, 30FPS 全速追蹤) | Imagiq 1190 (4K60 劇院級人像, 18-bit RAW) | Imagiq 890 (8K30 錄影) |

(資料來源與規格整合51)  
此外，搭載Arm Mali-G1 Ultra GPU的9500系列，圖形效能提升33%，光線追蹤能力翻倍（提升119%），並支援Unreal Engine 5.5的Nanite與MegaLights技術，帶來主機級別的遊戲畫質51。

### **寬頻設備與車用智慧座艙之跨領域延伸**

聯發科技將其深厚的Modem與SoC技術底蘊，延伸至固定無線存取（FWA）與車用電子領域：

* **5G FWA 與寬頻聯網：** 推出了T930、T900及T750等高階平台。T930是全球首款同步支援3GPP Release 18與Wi-Fi 8技術的平台。為解決雲端遊戲延遲，聯發科技更為CPE設備開發了基於AI的AQM（主動佇列管理）技術，顯著優化NVIDIA GeForce NOW的連線穩定度28。  
* **Dimensity Auto 車用平台：** MT2739是全球首款為車輛設計的5G-Advanced數據機，支援Release-17/18與衛星通訊，內建MMAI以自動優化隧道或地下室等切換場景的訊號穩定度49。此外，聯發科研發的C-X1智慧座艙平台，深度整合NVIDIA Blackwell GPU架構，能於車內執行多模態大型語言模型（LLM）與光線追蹤遊戲，重新定義了次世代車用娛樂與駕駛輔助系統50。

## **綜合結論**

系統單晶片（SoC）的開發是一項極度複雜的工程，從暫存器傳輸級（RTL）的邏輯定義、克服時脈域交接（CDC）的亞穩態挑戰，到最終實體設計的佈局繞線與時序收斂，每個環節都直接影響晶片的PPA指標。而在通訊晶片領域，硬體架構的演進更與3GPP標準緊密連動。5G NR中SDAP層的引入與PDCP功能的擴展，從根本上改變了基頻處理器的資料映射邏輯，賦予網路更高的靈活性以支援切片與多樣化的QoS。  
聯發科技的技術發展藍圖深刻地體現了上述軟硬體協同設計的哲學。透過M90數據機，聯發科技成功駕馭了5G-Advanced的極限載波聚合與上行增強技術，並率先將非地面網路（NTN）的衛星通訊能力商業化。更具前瞻性的是，MMAI（Modem AI）技術的導入，標誌著通訊基頻從靜態規則邁向「主動感知與預測」的Agentic AI時代，搭配UltraSave 4.0在物理層面的電壓與睡眠週期控制，達成了極致的能效比。同時，T300 RedCap RFSOC的誕生，展示了其在架構精簡與成本控制上的深厚功力；而天璣9500透過全大核架構與創新的CIM記憶體內運算NPU，樹立了旗艦行動平台的算力新標竿。綜觀其技術佈局，聯發科技不僅是通訊標準的追隨者，更已成為形塑未來6G網路與無所不在AI（Ubiquitous AI）基礎設施的關鍵推手。

#### **引用的著作**

1. What is a System on a Chip (SoC)? \- Synopsys, [https://www.synopsys.com/glossary/what-is-system-on-a-chip.html](https://www.synopsys.com/glossary/what-is-system-on-a-chip.html)  
2. SOC Design Flow \- VLSI Signal Processing Lab, EE, NCTU, [https://twins.ee.nctu.edu.tw/courses/soclab\_04/handout\_pdf/03\_SOC\_Design\_flow.pdf](https://twins.ee.nctu.edu.tw/courses/soclab_04/handout_pdf/03_SOC_Design_flow.pdf)  
3. VLSI IC Design Flow Explained | PDF | Computer Engineering \- Scribd, [https://www.scribd.com/document/874587046/Semicon-World-1697608307](https://www.scribd.com/document/874587046/Semicon-World-1697608307)  
4. End-to-End ASIC Design Flow Explained \- From Specification to Tape-Out, [https://techlabssemi.com/blogs/end-to-end-asic-design-flow-explained-from-specification-to-tape-out/](https://techlabssemi.com/blogs/end-to-end-asic-design-flow-explained-from-specification-to-tape-out/)  
5. 1.3 Introduction to the Digital Design Flow | Universalization of IC Design in CASS, [https://unic-cass.github.io/training/1.3-digital-design-flow-intro.html](https://unic-cass.github.io/training/1.3-digital-design-flow-intro.html)  
6. SoC-Architecture Components of a Baseband Single Chip Mobile Transceiver, [https://www.researchgate.net/figure/SoC-Architecture-Components-of-a-Baseband-Single-Chip-Mobile-Transceiver\_fig1\_221133251](https://www.researchgate.net/figure/SoC-Architecture-Components-of-a-Baseband-Single-Chip-Mobile-Transceiver_fig1_221133251)  
7. Design of Programmable Baseband Processors \- Search for publications in DiVA, [https://liu.diva-portal.org/smash/get/diva2:20611/FULLTEXT01.pdf](https://liu.diva-portal.org/smash/get/diva2:20611/FULLTEXT01.pdf)  
8. What is Register-Transfer-Level (RTL) Design? \- Synopsys, [https://www.synopsys.com/glossary/what-is-register-transfer-level-design.html](https://www.synopsys.com/glossary/what-is-register-transfer-level-design.html)  
9. RTL Design vs Physical Design: Which VLSI Career Path is Right for You? \- MOSart Labs, [https://mosartlabs.com/rtl-design-vs-physical-design-which-vlsi-career-path-is-right-for-you/](https://mosartlabs.com/rtl-design-vs-physical-design-which-vlsi-career-path-is-right-for-you/)  
10. ASIC Design Flow Overview | PDF | Formal Verification \- Scribd, [https://www.scribd.com/document/116395252/ASIC-Interview-Question-Answer-ASIC-Flow](https://www.scribd.com/document/116395252/ASIC-Interview-Question-Answer-ASIC-Flow)  
11. Expertise \- Aiken Logics, [https://www.aikenlogics.com/expertise.php](https://www.aikenlogics.com/expertise.php)  
12. VLSI Interview Prep Handbook by ProV Logic | PDF | Logic Gate | Digital Electronics \- Scribd, [https://www.scribd.com/document/845623608/Ultimate-VLSI-Interview-Preparation-Guide](https://www.scribd.com/document/845623608/Ultimate-VLSI-Interview-Preparation-Guide)  
13. ASIC Design Flow in VLSI Engineering Services – A Quick Guide \- eInfochips, [https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/](https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/)  
14. Physical Design for System-On-a-Chip, [https://cc.ee.ntu.edu.tw/\~eda/Course/IntroEDA06/Handouts/socpd-fig.pdf](https://cc.ee.ntu.edu.tw/~eda/Course/IntroEDA06/Handouts/socpd-fig.pdf)  
15. Clock Domain Crossing in SoC Designs | PDF | System On A Chip \- Scribd, [https://www.scribd.com/document/703210616/Clock-Domain-Crossing](https://www.scribd.com/document/703210616/Clock-Domain-Crossing)  
16. Understanding Clock Domain Crossing Issues \- Design And Reuse, [https://www.design-reuse.com/article/58990-understanding-clock-domain-crossing-issues/](https://www.design-reuse.com/article/58990-understanding-clock-domain-crossing-issues/)  
17. SoC Clock Domain Crossing – An Important Problem \- AnySilicon, [https://anysilicon.com/soc-clock-domain-crossing-an-important-problem/](https://anysilicon.com/soc-clock-domain-crossing-an-important-problem/)  
18. Low-Power Architectures for Clock Domain Crossing in SoC Design, [https://designthesolution.org/wp-content/uploads/2025/09/low-power-architectures-for-clock-domain-crossing-in-soc-design\_wish25\_aradhana\_kumari.pdf](https://designthesolution.org/wp-content/uploads/2025/09/low-power-architectures-for-clock-domain-crossing-in-soc-design_wish25_aradhana_kumari.pdf)  
19. Clock Domain Crossing Challenges in Latch Based Designs | DVCon Proceedings, [https://dvcon-proceedings.org/wp-content/uploads/clock-domain-crossing-challenges-in-latch-based-designs-presentation.pdf](https://dvcon-proceedings.org/wp-content/uploads/clock-domain-crossing-challenges-in-latch-based-designs-presentation.pdf)  
20. The 5 Biggest Challenges in Modern SoC Design (And How to Solve Them) \- Arteris, [https://www.arteris.com/blog/the-5-biggest-challenges-in-modern-soc-design-and-how-to-solve-them/](https://www.arteris.com/blog/the-5-biggest-challenges-in-modern-soc-design-and-how-to-solve-them/)  
21. 5G Protocol Stack | PHY, RRC, PDCP, SDAP, NAS, NGAP (3GPP Guide) \- 3GLTEInfo, [https://www.3glteinfo.com/5g/protocols/](https://www.3glteinfo.com/5g/protocols/)  
22. 5G NR PDCP & SDAP Layers \- NXG Connect, [https://www.nxgconnect.com/post/5g-nr-pdcp-sdap-layers](https://www.nxgconnect.com/post/5g-nr-pdcp-sdap-layers)  
23. 5G Toolbox and the 5G NR Protocol Layers \- MATLAB & Simulink \- MathWorks, [https://www.mathworks.com/help/5g/gs/5g-toolbox-and-the-5g-nr-physical-layer.html](https://www.mathworks.com/help/5g/gs/5g-toolbox-and-the-5g-nr-physical-layer.html)  
24. 5G \- Protocol Stack | PDF | Networks | Electronics \- Scribd, [https://www.scribd.com/document/513227658/5G-protocol-stack](https://www.scribd.com/document/513227658/5G-protocol-stack)  
25. 5G NR Radio Protocol Stack (Layer 2 and Layer 3\) \- Techplayon, [https://www.techplayon.com/5g-nr-radio-protocol-stack-layer-2-layer-3/](https://www.techplayon.com/5g-nr-radio-protocol-stack-layer-2-layer-3/)  
26. System Architecture \- 5G | ShareTechnote, [https://www.sharetechnote.com/html/5G/5G\_RadioProtocolStackArchitecture.html](https://www.sharetechnote.com/html/5G/5G_RadioProtocolStackArchitecture.html)  
27. MediaTek & 5G:, [https://www.docomo.ne.jp/binary/pdf/corporate/technology/rd/tech/5g/5GTBS2016\_TECH\_WORKSHOP\_MEDIATEK.pdf](https://www.docomo.ne.jp/binary/pdf/corporate/technology/rd/tech/5g/5GTBS2016_TECH_WORKSHOP_MEDIATEK.pdf)  
28. Powerful Connectivity Solutions \- MediaTek 5G, [https://www.mediatek.com/technology/5g](https://www.mediatek.com/technology/5g)  
29. 5G Progress | Global Innovation \- MediaTek, [https://www.mediatek.com/technology/5g/5g-progress](https://www.mediatek.com/technology/5g/5g-progress)  
30. 5G Solutions for Mobile, FWA & RedCap \- MediaTek, [https://www.mediatek.com/truly5g](https://www.mediatek.com/truly5g)  
31. MediaTek Introduces M90 5G-Advanced Modem with AI and 12Gbps Peak Speed, [https://www.mediatek.com/press-room/mediatek-introduces-m90-5g-advanced-modem-with-ai-and-12gbps-peak-speed](https://www.mediatek.com/press-room/mediatek-introduces-m90-5g-advanced-modem-with-ai-and-12gbps-peak-speed)  
32. Press Room | Latest News & Press Releases | 5G \- MediaTek, [https://www.mediatek.com/press-room/tag/5g](https://www.mediatek.com/press-room/tag/5g)  
33. MediaTek Expands Cloud-to-Edge Leadership with Next-Generation Connectivity and AI at MWC 2025, [https://www.mediatek.com/press-room/mediatek-expands-cloud-to-edge-leadership-with-next-generation-connectivity-and-ai-at-mwc-2025](https://www.mediatek.com/press-room/mediatek-expands-cloud-to-edge-leadership-with-next-generation-connectivity-and-ai-at-mwc-2025)  
34. MediaTek Introduces M90 5G-Advanced Modem with AI and 12Gbps Peak Speed, [https://www.stocktitan.net/news/MDTKF/media-tek-introduces-m90-5g-advanced-modem-with-ai-and-12gbps-peak-ijekxxc9s5sg.html](https://www.stocktitan.net/news/MDTKF/media-tek-introduces-m90-5g-advanced-modem-with-ai-and-12gbps-peak-ijekxxc9s5sg.html)  
35. In-Depth: MediaTek M90 5G-Advanced Modem – Specifications and Features Explained, [https://cyberraiden.wordpress.com/2026/02/19/in-depth-mediatek-m90-5g-advanced-modem-specifications-and-features-explained/](https://cyberraiden.wordpress.com/2026/02/19/in-depth-mediatek-m90-5g-advanced-modem-specifications-and-features-explained/)  
36. M90 5G Modem \- MediaTek, [https://www.mediatek.com/technology/5g/m90-modem](https://www.mediatek.com/technology/5g/m90-modem)  
37. Press Room | Latest News & Press Releases | Connectivity \- MediaTek, [https://www.mediatek.com/press-room/tag/connectivity](https://www.mediatek.com/press-room/tag/connectivity)  
38. 5G Modem | Ultra-Fast, Power-Efficient 5G \- MediaTek, [https://www.mediatek.com/technology/5g/5g-modem](https://www.mediatek.com/technology/5g/5g-modem)  
39. I Tested 5G Modems on Qualcomm and MediaTek Phones to Find Which Performs Better in India | Beebom Gadgets, [https://gadgets.beebom.com/stories/i-tested-5g-modems-on-qualcomm-and-mediatek-phones-to-find-which-performs-better](https://gadgets.beebom.com/stories/i-tested-5g-modems-on-qualcomm-and-mediatek-phones-to-find-which-performs-better)  
40. MediaTek's latest 5G chip Dimensity 700 is launched, with octa-core CPU architecture \+ UltraSave power saving technology \- EEWORLD, [https://en.eeworld.com.cn/news/qrs/eic516219.html](https://en.eeworld.com.cn/news/qrs/eic516219.html)  
41. MediaTek Unveils RedCap Solutions to Deliver 5G Data Rates and Impressive Power Efficiency to a Broad Range of IoT Devices, [https://www.mediatek.com/press-room/mediatek-unveils-redcap-solutions-to-deliver-5g-data-rates-and-impressive-power-efficiency-to-a-broad-range-of-iot-devices](https://www.mediatek.com/press-room/mediatek-unveils-redcap-solutions-to-deliver-5g-data-rates-and-impressive-power-efficiency-to-a-broad-range-of-iot-devices)  
42. MediaTek T300 \- 5G RedCap IoT Modem SoC, [https://www.mediatek.com/products/iot/modem-based-iot/mediatek-t300](https://www.mediatek.com/products/iot/modem-based-iot/mediatek-t300)  
43. MediaTek introduces 5G RedCap connectivity solutions for IoT, wearables and data-cards, [https://www.mediatek.com/tek-talk-blogs/mediatek-introduces-5g-redcap-connectivity-solutions-for-iot-wearables-and-data-cards](https://www.mediatek.com/tek-talk-blogs/mediatek-introduces-5g-redcap-connectivity-solutions-for-iot-wearables-and-data-cards)  
44. Quectel announces RG255G MediaTek-based 5G RedCap module, [https://www.quectel.com/news-and-pr/5g-redcap-module-rg255g-modem-rf-soc-mediatek-ultrasave/](https://www.quectel.com/news-and-pr/5g-redcap-module-rg255g-modem-rf-soc-mediatek-ultrasave/)  
45. MediaTek Unveils T300 5G RedCap Platform for IoT and Extremely Low Power Devices, [https://www.mediatek.com/press-room/mediatek-unveils-t300-5g-redcap-platform-for-iot-and-extremely-low-power-devices](https://www.mediatek.com/press-room/mediatek-unveils-t300-5g-redcap-platform-for-iot-and-extremely-low-power-devices)  
46. 5G RedCap Modem \- MediaTek, [https://www.mediatek.com/technology/5g/5g-redcap-modem](https://www.mediatek.com/technology/5g/5g-redcap-modem)  
47. 5G RedCap Hardware Guide – Chipsets, Modules, Routers & Devices, [https://5gredcap.co.uk/5g-redcap-hardware/](https://5gredcap.co.uk/5g-redcap-hardware/)  
48. MediaTek T300, [https://mediatek-marketing.files.svdcdn.com/production/documents/Infographics/MediaTek-T300\_Infographic\_Final\_0124.pdf?dm=1709068272](https://mediatek-marketing.files.svdcdn.com/production/documents/Infographics/MediaTek-T300_Infographic_Final_0124.pdf?dm=1709068272)  
49. Dimensity Auto Connect MT2739 \- 5G Modem \- MediaTek, [https://www.mediatek.com/tek-talk-blogs/dimensity-auto-connect-mt2739](https://www.mediatek.com/tek-talk-blogs/dimensity-auto-connect-mt2739)  
50. MediaTek Unveils Flagship Dimensity Auto Platforms, Defining the Future of The Intelligent Cockpit, [https://www.mediatek.com/press-room/mediatek-unveils-flagship-dimensity-auto-platforms-defining-the-future-of-the-intelligent-cockpit](https://www.mediatek.com/press-room/mediatek-unveils-flagship-dimensity-auto-platforms-defining-the-future-of-the-intelligent-cockpit)  
51. RESEARCH NOTE: MediaTek Launches New Dimensity 9500 Flagship SoC for Smartphones \- Moor Insights & Strategy, [https://moorinsightsstrategy.com/research-notes/mediatek-launches-new-dimensity-9500-flagship-soc-for-smartphones/](https://moorinsightsstrategy.com/research-notes/mediatek-launches-new-dimensity-9500-flagship-soc-for-smartphones/)  
52. Dimensity 9500 | Flagship 5G Mobile Chipset \- MediaTek, [https://www.mediatek.com/products/smartphones/mediatek-dimensity-9500](https://www.mediatek.com/products/smartphones/mediatek-dimensity-9500)  
53. MediaTek Dimensity 9500 – Flagship 5G Agentic AI Platform, [https://www.mediatek.com/dimensity-9500](https://www.mediatek.com/dimensity-9500)  
54. MediaTek Dimensity 9500: Powering Next-Gen Flagship Smartphones with On-Device GenAI, 100 TOPS, Gaming Excellence \- Counterpoint Research, [https://counterpointresearch.com/en/insights/mediatek-dimensity-9500-powering-powering-next-gen-smartphones](https://counterpointresearch.com/en/insights/mediatek-dimensity-9500-powering-powering-next-gen-smartphones)  
55. MediaTek Dimensity 9500 Unleashes Best-in-Class Performance, AI Experiences, and Power Efficiency for the Next Generation of Mobile Devices, [https://www.mediatek.com/press-room/mediatek-dimensity-9500-unleashes-best-in-class-performance-ai-experiences-and-power-efficiency-for-the-next-generation-of-mobile-devices](https://www.mediatek.com/press-room/mediatek-dimensity-9500-unleashes-best-in-class-performance-ai-experiences-and-power-efficiency-for-the-next-generation-of-mobile-devices)  
56. Dimensity 9500s | Flagship 3nm 5G Smartphone Chip \- MediaTek, [https://www.mediatek.com/products/smartphones/mediatek-dimensity-9500s](https://www.mediatek.com/products/smartphones/mediatek-dimensity-9500s)  
57. MediaTek Dimensity 9500s – Specification Deep Dive: 5G, Wi-Fi 7, Bluetooth 5.4, GNSS & More \- Cyber Raiden, [https://cyberraiden.wordpress.com/2026/03/05/mediatek-dimensity-9500s/](https://cyberraiden.wordpress.com/2026/03/05/mediatek-dimensity-9500s/)  
58. MediaTek Dimensity 9400 Plus vs MediaTek Dimensity 9200 Plus : Specifications and Benchmark Comparison \- 91Mobiles, [https://www.91mobiles.com/processor/mediatek-dimensity-9400-plus-vs-mediatek-dimensity-9200-plus](https://www.91mobiles.com/processor/mediatek-dimensity-9400-plus-vs-mediatek-dimensity-9200-plus)  
59. MediaTek to Empower the Agentic AI Era with Edge-to-Cloud Tech at Computex 2026, [https://www.mediatek.com/press-room/mediatek-to-empower-the-agentic-ai-era-with-edge-to-cloud-tech-at-computex-2026](https://www.mediatek.com/press-room/mediatek-to-empower-the-agentic-ai-era-with-edge-to-cloud-tech-at-computex-2026)  
60. RMM-G4 T900 Series 5GeSIMGNSSM.2NSASA \- Compal, [https://www.compal.com/5g/products/RMM-G4/](https://www.compal.com/5g/products/RMM-G4/)

[image1]: <data:image/png;base64,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>