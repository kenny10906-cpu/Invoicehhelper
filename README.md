# 智慧發票多功能掃描 App (Intelligent Multi-Functional Invoice Scanner App)

[![Android](https://img.shields.io/badge/Platform-Android%2014.0%20%28API%2034%29-green.svg)](#2-硬體與軟體環境)
[![Gradle](https://img.shields.io/badge/Build%20Tool-Gradle%20Kotlin%20DSL-blue.svg)](#2-硬體與軟體環境)
[![AI-Engine](https://img.shields.io/badge/Edge%20AI-Google%20ML%20Kit%20%2F%20100%25%20Offline-orange.svg)](#6-模型或演算法說明)

本專案是一款基於**嵌入式電腦視覺與邊緣運算（Edge AI）**架構開發的智慧發票多功能掃描與自動化記帳 Android 應用程式。系統具備「快慢雙軌」分流處理機制[cite: 8]，針對完好電子發票提供毫秒級的極速解碼[cite: 8]；針對受損、褪色或傳統長條型發票，則內建基於數位影像處理（DIP）與階層式權重判定邏輯的 **AI 文字救援機制 (Fallback to OCR)**[cite: 8]，完美解決日常記帳的半自動化痛點[cite: 8]。

---

## 1. 專題故事與應用情境
在數位轉型時代，使用者在實際使用手機掃描發票記帳時，經常面臨以下三大痛點[cite: 8]：
1. **硬體毀損難辨識**：部分熱感應紙（如加油站發票）常因印字頭斷針導致 QR Code 定位點受損[cite: 8]，導致傳統掃描器直接判定「查無此條碼」[cite: 8]。
2. **傳統與電子發票並存**：長條形傳統發票完全無條碼[cite: 8]，使用者必須在「掃描」與「手動輸入」模式間頻繁切換[cite: 8]。
3. **數字陷阱干擾**：發票上充斥著電話、店號、序號等數字[cite: 8]，一般 AI 辨識時常發生「誤抓」（例如將流水序號 `1260` 當成結帳金額）[cite: 8]。

### 💡 我們的解決方案
本系統不僅是條碼掃描器，更是具備邏輯判斷能力的記帳助手[cite: 8]：
* **即時極速模式**：針對完好電子發票，利用本地端邊緣運算進行 `< 100ms` 的瞬時解碼[cite: 8]。
* **AI 文字救援 (Fallback)**：當條碼受損或遇到傳統發票時，一鍵啟動「影像增強 + OCR」技術，將紙面文字轉化為結構化數據[cite: 8]。
* **智慧去重與記帳**：自動以發票號碼進行本地資料庫比對[cite: 8]，防止同一張單據重複入帳，並直接連結財務管理[cite: 8]。

---

## 2. 系統需求
為確保本嵌入式電腦視覺系統能流暢運行且不發生記憶體溢位 (OOM)，環境需符合以下規範[cite: 8]：
* **處理器**：分配至少 4 核心 CPU，需具備向量運算加速指令集（以利本地推論加速）[cite: 8]。
* **記憶體配置**：系統占用控制在 4GB RAM 以內，JVM 精靈進程配置至少 2048MB（`org.gradle.jvmargs=-Xmx2048m`）[cite: 6, 8]，虛擬機 Heap Size 設為 512MB[cite: 8]。
* **影像輸入**：支援 720p (1280×720) 或 1080p (1920×1080) 像素之自動對焦鏡頭[cite: 8]。

---

## 3. 硬體與軟體環境

### 硬體環境
* **終端設備**：Android 實體移動設備（智慧型手機）或 Android Studio 虛擬裝置 (AVD)[cite: 8]。

### 軟體環境
* **作業系統**：Android 14.0 (API Level 34) 以上[cite: 8]。
* **相機架構**：Android CameraX API（實作 `STRATEGY_KEEP_ONLY_LATEST` 影格分析策略）[cite: 8]。
* **邊緣 AI 引擎**：Google ML Kit SDK (On-device 本地端版，100% 離線運作)[cite: 8]。
  * Barcode Scanning API (QR Code 快速解碼)[cite: 8]
  * Text Recognition API (繁體中文 OCR 救援)[cite: 8]
* **主要開發語言**：Java / Kotlin 混合架構[cite: 8]。
* **軟體建構工具**：Gradle Kotlin DSL (`build.gradle.kts`[cite: 5], `settings.gradle.kts`, `gradle.properties`[cite: 6])。

---

## 4. 安裝方式

1. **複製專案庫 (Clone Repository)**：
```bash
   git clone <請替換為您的 GitHub 專案網址>
   cd <專案資料夾名稱>
1.設定本地開發環境：
專案會自動偵測 local.properties 以載入您本地的 Android SDK 路徑（註：該檔案與 .gradle、/build 等編譯暫存路徑已被列入 .gitignore 中）。請確保路徑正確：

Properties


sdk.dir=C\:\\Users\\YourUsername\\AppData\\Local\\Android\\Sdk
2.同步 Gradle 依賴：
使用 Android Studio 開啟此專案。系統將自動讀取 settings.gradle.kts，透過 Foojay Toolchain 解析器配置 JDK 環境，並從 Google 與 Central 倉庫下載所需之 ML Kit 與 CameraX 套件。

## 5.執行方式

方案 A：使用 Android Studio 部署（推薦）
將 Android 實體手機連接至電腦並開啟「USB 除錯」，或啟動 AVD 模擬器。

在 Android Studio 上方工具列選定 app 模組與目標裝置。

點擊 Run (▶) 按鈕，系統將自動進行編譯並部署安裝至裝置。

方案 B：使用命令列工具建構 APK
本專案隨附 Gradle Wrapper 腳本，可在未預裝 Gradle 的命令列環境下直接執行：

Windows 系統：

DOS


gradlew.bat assembleDebug
Linux / macOS 系統：

Bash


chmod +x gradlew
./gradlew assembleDebug
建構完成後的 APK 檔案將存放在 app/build/outputs/apk/debug/。

6. 資料集說明

本專題之資料集主要由團隊組員自行拍攝之真實生活消費發票構成（具備 Ground Truth 真實標記以供比對校驗），樣本具備以下三大特徵分類：

電子發票樣本：包含完整雙 QR Code 標頭，用於驗證「財政部官方 77 碼校驗演算法」。

傳統型發票樣本：長條形排版、完全無條碼，且具備「品名與金額左右分離」之挑戰。

高雜訊與嚴重瑕疵樣本：包含強光反光、墨跡褪色、縐褶，以及充斥流水序號、店號等隨機 8 位數干擾碼的單據。

(本軟體原始碼內附有典型測試影像：1.jpg、2.jpg 與 invoice_qr.png 供開發除錯與功能測試使用

7. 模型或演算法說明本系統的核心邊緣運算邏輯包含以下三大關鍵演算法：

A. 數位影像處理 (DIP) 前處理針對「褪色」與「縐褶」發票，系統在進入 OCR 前會對影格影像進行以下標準化增強：灰階化與去飽和度：套用 ColorMatrix 將補色通道和飽和度設為 0，消除環境光產生的色偏。動態對比度增益：實作 3.0x 的對比度乘數（Contrast Gain），將淡化墨跡強制拉回高對比訊號。亮度修正與歸一化：套用 -100f 亮度偏移，有效抑制強光反光產生的死白區域，提升邊緣字元可讀性。
B. 行坐標整歸類演算法 (Row-Alignment Algorithm)為解決傳統發票「品名在左側、金額在右側」導致 OCR 縱向讀取順序錯亂、行距折疊的問題：掃描 OCR 輸出的所有文字塊（TextBlocks）與文字行（Lines），並計算其中心點 Y 軸座標。若兩文字塊的縱向高度差 $\Delta Y < 30\text{px}$，則強迫歸類為「同一行」，藉此在資料流中完美重建橫向排版數據。
C. 階層式權重金額判定與過濾邏輯利用正則表達式（Regex）與權重狀態機排除非金額數字（如發票號碼、電話、序號等數字陷阱）：Level 1 (最高優先級)：精準鎖定「小計、現金、總計、總計金額」關鍵字，抓取其正右方或同一水平 Y 軸對齊之數字。Level 2 (次級權重)：若無關鍵字，自動搜尋帶有 $、NT$ 或 元 貨幣標籤之數字。過濾機制：自動比對數字位數與前後文字脈絡，精準剔除 8 位數發票號、序號、交易時間與店家電話。

8. 測試結果

A.實測效能數據 (Performance Benchmark)在 Android Studio AVD (模擬中階移動端 CPU + GPU 加速) 環境下實測效能如下：預覽影格率：24 ~ 30 FPS (操作體感極致流暢，無明顯遲滯)。即時 QR Code 解析延遲：85 毫秒 (完成財政部 77 碼隨機碼與總金額瞬時校驗)。AI 深度文字解析延遲：1445 毫秒 (約 1.4 秒) (含 DIP 影像增強、OCR 本地推論與權重邏輯決策)。離線運作能力：100% 離線運作，在完全斷網狀態下所有演算法運行正常。
B. 辨識正確率統計 (Accuracy Analysis)針對標準電子發票、受損發票、傳統發票及高雜訊瑕疵單據進行黑箱測試與人工覆核：發票號碼正確率：100%。期別月份正確率：100%（成功透過全域狀態暫存器 Cache 解決影格跳動閃爍消失問題）。結帳金額抓取正確率：90%（10張典型樣本中成功排除絕大部分序號陷阱，成功杜絕「序號 1260」與「條碼編號 9955」等常見誤抓干擾，僅在極限高雜訊瑕疵樣本產生 1 筆誤判）。
