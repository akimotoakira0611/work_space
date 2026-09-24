# System Tools - PCIe Margin 自動分析

## 全新升級功能
1. **動態 Criteria 設定 (可調參數)：** 介面開放設定 `Min Vref`、`Min UI` 與 `Target Runs`，預設為 AMD SP7 Base 規範。SI 工程師可依專案需求彈性調整判定標準。
2. **Bifurcation 強制防呆與報警：** 系統自動比對 JSON `POST MARGIN` 中的 `Device Width` 與實際解析到的 Lane 數量。若發生不匹配（如識別 x16 卻只跑出 8 條數據），Overall 結果強制轉為 FAIL 並顯示嚴重警告。
3. **HTML 報告即時匯出與下載：** 分析完成後不僅會自動彈出擁有頂級 Dashboard 介面（PASS/FAIL 率、最差眼圖數據看板）的報告，主畫面更會顯示「Download HTML Report」按鈕，讓工程師能將獨立的單檔 HTML 報告留底存查或寄送給客戶。
