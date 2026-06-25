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
