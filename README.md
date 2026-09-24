# System Tools 分析與管理網頁

本專案整合了 PCIe Margin JSON 分析工具，透過前端 JavaScript 即時處理大量測試日誌。

## PCIe Gen6 Margin 分析工具核心功能
1. **動態 Criteria 與資料量防呆：**
   自動驗證測試筆數是否達到 20 Runs 最低門檻。不滿足時顯示視覺化警告。
2. **Bifurcation 嚴格通道驗證：**
   自動讀取 JSON 中的 Device Width（如 x16, x8），並逐一比對每個 Run 產出的 Lane 數量。若數量不符（例如拆分為 x8 卻少於 8 條資料），系統將觸發紅色嚴重警告，防止誤判。
3. **高階資料視覺化 Dashboard：**
   致敬業界專業報告排版，分析後於新分頁彈出 Dashboard。包含頂部資訊看板（PASS/FAIL 總數、Worst Eye 指標）與 Top 5 Worst Case 分析圖表，提供 SI 工程師清晰的除錯指標。
