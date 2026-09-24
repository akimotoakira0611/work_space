# PCIe Margin 自動分析與判定工具

本專案為針對 AMDXIO 工具產生的 PCIe Margin JSON 測試報告所開發之前端分析網頁。工具利用純 JavaScript 於使用者本地端瀏覽器即時處理與渲染，無需依賴後端伺服器或資料庫。

## 核心分析邏輯

### 1. 資料量自動偵測與警告
根據 AMD 測試規範，基礎 Margin Search 的門檻為交互測試達 20 Runs (System #1 包含 2 EPs $\times$ 5 runs，System #2 包含 2 EPs $\times$ 5 runs)[cite: 8]。
當匯入的測試數據陣列總長度小於 20 筆時，前端介面將自動觸發 `display: block` 顯示紅色警告標語 `⚠️ 數據總量不足 20 筆，僅作參考`，提醒測試者資料集尚未達到 AMD 最低收斂標準。

### 2. Criteria 嚴格判定閘
測試的 Criteria 判定採用 20 Runs 的最低標準：
*   **Vref Up / Vref Down:** $\ge$ 2 offsets[cite: 8]
*   **UI Right-Left:** $\ge$ 1 ui step[cite: 8]

程式透過雙層迴圈掃描所有檔案內的 `Margin Data -> Per Lane Data`。當**任一 Run 的任一 Lane** 其數值未達上述標準，全域變數 `overallPass` 將轉為 `false`，並在分析摘要區塊中宣告整體結果為 **FAIL**。各別的 Lane 也會在資料表中依據此標準標記 `PASS` 或 `FAIL`。

### 3. 全域最差值篩選 (Worst-Case Identification)
為了提供 SI 工程師精確的除錯指標，程式內建「眼圖面積指標」計算公式：
$$Area = (Vref_{Up} + Vref_{Down}) \times UI_{Right-Left}$$
在解析 JSON 結構的過程中，程式會動態追蹤並覆寫全域最小的 Area 數值。掃描結束後，將抓取到的最低面積數據點識別為「最差通道 (Worst Lane)」，並擷取其對應的 Run 編號、Lane ID、電壓與相位偏移值，獨立顯示於報告頂部，同時以深紅色高亮該列數據。

## 使用方式
1. 點擊網頁中的「選擇檔案」按鈕。
2. 支援 `multiple` 多選，可一次選取多份由 AMDXIO 生成的 `.json` 報告檔。
3. JavaScript 的 `FileReader` API 將非同步讀取內容，並在全數解析完畢後自動渲染出摘要區塊與資料表。
