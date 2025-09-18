# platformAnalze
平台網頁爬蟲分析

#  三大電商平台「洗衣機」價格比較分析專題

本專案為期末專題，目標為從三大電商平台（PChome、東森購物、MOMO購物網）爬取「洗衣機」的相關產品資訊，並將資料儲存至 MongoDB，進行比價分析與視覺化展示，幫助使用者了解不同平台的售價差異與熱門商品。

---

##  功能說明

-  從三大平台爬取「洗衣機」商品資訊（名稱、價格）
-  資料儲存至 MongoDB（或以 CSV → JSON）
-  資料清洗與商品篩選（例：國際牌直立洗衣機、樂金滾筒洗衣機）
-  商品價格視覺化圖表（柱狀圖、折線圖）
-  同品項商品在各平台售價比較

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
  ```
---

## 🔧 執行方式（How to Run）

### 1️⃣ 安裝必要套件

```bash
pip install -U pymongo requests beautifulsoup4 pandas matplotlib lxml
```

> ✅ 若在 Jupyter Notebook 中執行，也可直接在 cell 中輸入：
>
> ```python
> !pip install -U pymongo requests beautifulsoup4 pandas matplotlib lxml
> ```

---

### 2️⃣ 啟動 MongoDB 本地資料庫

請確認本機端的 MongoDB 已啟動，並已建立帳號與密碼：

- Host：`localhost`
- Port：`27017`
- Username：`tina`
- Password：`0000`

---

### 3️⃣ 執行爬蟲與資料儲存流程

打開 `Final期末專題-最終版-Demo.ipynb`，依序執行以下 cell：

#### 🔹 PChome 資料爬取
```python
pchome('洗衣機', client['pchome'], 'washingMachine')
```

#### 🔹 東森購物資料爬取
```python
etmall('洗衣機', client['etmall'], 'washingMachine')
```

#### 🔹 MOMO 資料爬取（HTML 解析）
```python
createCsv('MOMOWashingMachine')
momo('洗衣機', 32, 'MOMOWashingMachine')
csv2json('MOMOWashingMachine')
```

---

### 4️⃣ 資料查詢與分析

#### 🔹 建立 DataFrame
```python
df1 = callPchome("洗衣機", "washingMachine", "國際牌", "直立洗衣機")
df2 = callEtmall("洗衣機/脫水機/乾衣機", "washingMachine", "國際牌", "直立洗衣機")
df3 = callMOMO("washingMachine", "國際牌", "直立洗衣機")
```

#### 🔹 顯示結果
```python
pd.concat([df1, df2, df3], axis=1, join="outer")
```

---

### 5️⃣ 資料視覺化（價格與數量比較）

#### 🔹 商品數量柱狀圖
```python
callTotal("洗衣機/國際牌/直立洗衣機", len(df1.品項), len(df2.品項), len(df3.品項))
```

#### 🔹 同商品價格折線圖
```python
plt.plot(...)  # 比較三平台售價
```

---
