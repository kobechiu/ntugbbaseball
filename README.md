# ntugbbaseball 棒球訓練手冊

**網站:<https://kobechiu.github.io/ntugbbaseball/>**

把教練整理的訓練項目、隊內訓練菜單,以及 Driveline 打擊課表整理成手機好用的網頁,訓練時可以快速查到「這個項目在練什麼、影片在哪、今天練什麼」。

| 分頁 | 內容 |
|---|---|
| 項目庫 | 所有訓練項目,依目的、部位、來源篩選,含技術要點與影片 |
| 隊內菜單 | 教練的訓練流程(大流程、打擊、內野、外野),一步步列出該練的項目 |
| Driveline | 休賽季/賽季中週期課表,點某天就看到當天課表,可打勾記錄 |
| 知識 | Driveline 評估與原理說明(中文翻譯,附英文原文) |
| 回饋・新增 | 回饋意見、推薦新項目,送出到 Google 表單 |

## 我想參與,該怎麼做?

### 隊友:回饋或推薦項目
直接在網站的「回饋・新增」分頁填寫即可,不需要 GitHub 帳號。在項目詳細頁按「回饋這個項目」會自動帶入項目名稱。

### 資料管理者:更新訓練資料
所有資料都在 **`棒球訓練2026_整合.xlsx`**。更新流程完全在 GitHub 網頁上完成,不用安裝任何東西:

1. 下載最新的 `棒球訓練2026_整合.xlsx`(點檔名 → 右上角下載按鈕)
2. 用 Excel 修改並存檔(**檔名不要改**)
3. 回到 repo 首頁 → **Add file → Upload files** → 拖入檔案 → 寫一句修改說明 → **Commit changes**
4. 約 1–2 分鐘後網站自動更新。到 **Actions** 分頁點最新一筆,可以看到「產生結果」與資料檢查提醒(例如編號重複、影片連結不是網址)

> Excel 無法多人同時編輯合併。請**先下載最新版再改**,改完盡快上傳;同一時間只讓一個人改。

### 開發者:修改網頁或程式
Fork 本 repo,修改後發 Pull Request。本機預覽:

```bash
pip install openpyxl
python3 tools/build.py          # 產生 index.html,並列出資料檢查結果
python3 -m http.server 8000     # 開 http://localhost:8000(YouTube 內嵌需用 http 開啟)
```

不要直接修改 `index.html`,它是產生出來的檔案(已列入 `.gitignore`)。

## Excel 工作表說明

| 工作表 | 網頁用途 | 填寫規則 |
|---|---|---|
| 訓練項目 | 項目庫(教練項目) | 編號不可重複;**部位**只能填 `上肢`/`下肢`/`全身`;**目的**用「詞條」裡的分類;影片連結要是完整網址 |
| 詞條 | 目的分類與排列順序 | |
| 菜單 | 隊內菜單 | 一列一個步驟;**項目編號**用頓號分隔(如 `41、52、61`),多個代表「任選」;**分支**留空 = 主步驟,填 `進階`/`延伸`/`學姊` 等 = 附在上一個主步驟下的分支 |
| 大流程、打擊訓練組合、內野守備組合、外野守備組合 | 不直接使用 | 教練原始流程圖,僅供參考;網頁讀的是「菜單」 |
| DL_動作 | 項目庫(Driveline 動作) | 編號格式 `D-01`;**相關項目**可填教練項目編號,網頁會互相連結 |
| DL_課表類型、DL_課表、DL_週期 | Driveline 課表 | 課表代碼需對應;週期中填 `休息` 代表休息日 |
| DL_說明 | 知識分頁 | 段落間空一行;`【標題】` 會顯示為粗體小標 |
| 器材 | Driveline 器材與替代方案 | 填了**替代方案**,課表上會顯示替代器材 |
| 設定 | Google 表單網址 | 修改表單題目後重新上傳任一檔案即可同步;新題目標題需含網頁欄位的關鍵字(見 `tools/build.py` 的 `FORM_FIELDS`) |

## 專案結構

```
棒球訓練2026_整合.xlsx     所有資料(唯一資料來源)
tools/build.py            讀 Excel → 檢查資料 → 產生 index.html
tools/template.html       網頁模板(HTML/CSS/JS,無外部套件)
tools/forms_cache.json    Google 表單題目的備份,無法連線時使用
.github/workflows/        推送到 main 後自動產生並發布網站
```

## 授權與資料來源

- **程式碼**(`tools/`、`.github/`):[MIT License](LICENSE)
- **教練整理的訓練資料**(訓練項目、菜單等):[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.zh-hant),轉用請註明出處
- **Driveline 內容**(DL_ 開頭的工作表、知識分頁):翻譯整理自 Driveline Baseball《6 Week In-Season & Off-Season Hitting Program》,著作權屬於 [Driveline Baseball](https://www.drivelinebaseball.com/),**不在上述授權範圍內**。請勿上傳原始 PDF。
- 各影片連結的著作權屬於原作者。
