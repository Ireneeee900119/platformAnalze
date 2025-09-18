# platformAnalze
平台網頁爬蟲分析

#  三大電商平台「洗衣機」價格比較分析專題

本專案為期末專題，目標為從三大電商平台（PChome、東森購物、MOMO購物網）爬取「洗衣機」的相關產品資訊，並將資料儲存至 MongoDB，進行比價分析與視覺化展示，幫助使用者了解不同平台的售價差異與熱門商品。

---

##  功能說明

- 🔍 從三大平台爬取「洗衣機」商品資訊（名稱、價格）
- 🗃️ 資料儲存至 MongoDB（或以 CSV → JSON）
- 🧾 資料清洗與商品篩選（例：國際牌直立洗衣機、樂金滾筒洗衣機）
- 📊 商品價格視覺化圖表（柱狀圖、折線圖）
- 🔁 同品項商品在各平台售價比較

---

##  使用技術（Tech Stack）

| 類別        | 技術說明                     |
|-------------|------------------------------|
| 語言        | Python 3.8                   |
| 開發環境    | Jupyter Notebook             |
| 爬蟲工具    | `requests`, `BeautifulSoup` |
| 資料處理    | `pandas`, `json`, `csv`     |
| 資料庫      | MongoDB (`pymongo`)          |
| 資料視覺化  | `matplotlib`, `numpy`        |

---

##  資料流程圖

```mermaid
graph TD;
  A[關鍵字輸入：洗衣機] --> B1[PChome爬蟲 API];
  A --> B2[ETMall東森爬蟲 API];
  A --> B3[MOMO爬蟲 HTML];

  B1 --> C1[JSON 儲存至 MongoDB];
  B2 --> C2[JSON 儲存至 MongoDB];
  B3 --> D1[CSV → JSON → MongoDB];

  C1 & C2 & D1 --> E[統一格式轉為 DataFrame];
  E --> F[關鍵字篩選（品牌 / 類型）];
  F --> G[視覺化比較圖表]

執行方式

1. 安裝套件
pip install -U pymongo requests beautifulsoup4 pandas matplotlib

2. 啟動 MongoDB（需本機安裝）

確保本地 MongoDB 啟動，帳號密碼為：
	•	帳號：tina
	•	密碼：0000
	•	連線參數：localhost:27017

3. 執行 Notebook（或 Python Script）

步驟：
	•	使用 pchome() 函數爬取 PChome 商品
	•	使用 etmall() 函數爬取東森商品
	•	使用 momo() → csv2json() 取得 MOMO 商品資料
	•	使用 callPchome()、callEtmall()、callMOMO() 轉為 DataFrame
	•	執行 callTotal() 與 plt.plot() 進行可視化分析
注意事項
	•	MOMO 網站需以 HTML 方式解析，解析元素為 .prdInfoWrap、.prdName、.price
	•	PChome 與 ETMall 則可直接使用 JSON API 呼叫
	•	若 MongoDB 未開啟或權限設定錯誤，請先檢查帳密與連線狀態
	•	資料存在重複與不同命名（例如 “變頻”/“定頻”）情況，需自行進行清理或比對
