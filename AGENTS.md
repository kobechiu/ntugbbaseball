# AGENTS.md

給 AI 開發工具(Claude Code、Cursor、Copilot、Codex 等)的專案說明。人類協作者請看 [README.md](README.md)。

## 專案是什麼

臺大女棒的訓練手冊網站,給隊員在訓練時用手機查訓練項目、菜單、影片。
網站是單一靜態 HTML,由 `tools/build.py` 讀取 Excel 產生,推送到 `main` 後 GitHub Actions 自動建置並發布到 GitHub Pages。

## 檔案結構

```
棒球訓練2026_整合.xlsx     唯一的資料來源(訓練項目、菜單、Driveline 課表、設定)
tools/build.py            讀 Excel → 資料檢查 → 把 JSON 注入模板 → 輸出 index.html
tools/template.html       網頁本體:HTML + CSS + JS 全在這一個檔案
tools/forms_cache.json    Google 表單題目結構的備份(build.py 自動更新)
.github/workflows/deploy.yml  建置與發布
```

`index.html` 是產生出來的檔案,已列入 `.gitignore`。

## 常用指令

```bash
pip3 install openpyxl
python3 tools/build.py          # 產生 index.html,最後會列出「資料檢查」
python3 -m http.server 8000     # 預覽 http://localhost:8000
```

## 必須遵守的規則

1. **不要修改 `index.html`**。要改網頁就改 `tools/template.html`,要改資料處理就改 `tools/build.py`,要改內容就改 Excel。
2. **維持單一檔案、不加外部依賴**:不引入框架、CDN、npm 套件、額外的建置步驟。只用原生 HTML/CSS/JavaScript。
3. **以手機為主**:隊員大多在球場用手機看。改完要用 390px 寬的畫面檢查,觸控目標不要太小。
4. **介面文字用繁體中文**(台灣用語)。英文專有名詞保留原文並附中文。
5. **支援深色模式**:顏色一律用 `:root` 裡的 CSS 變數,不要寫死色碼。
6. **防止 XSS**:在 JS 裡組 HTML 字串時,所有來自資料或使用者的值都要經過 `esc()`。
7. **Excel 是唯一資料來源**:
   - `build.py` 依照「標題列的欄位名稱」讀取工作表。新增欄位或工作表時,要同時更新 `build.py` 和 README 的「Excel 工作表說明」表格。
   - 用 openpyxl 修改 Excel 時,要載入原檔再修改,不要重新建立整份活頁簿(會遺失其他工作表和格式)。
   - 資料規則:「部位」只能是 `上肢`/`下肢`/`全身`;訓練項目編號不可重複;「菜單」的項目編號用頓號 `、` 分隔;「分支」欄空白代表主步驟。
8. **不能放進 repo 的東西**:PDF、影片、`.pages` 等原始素材(有版權或檔案太大,GitHub 單檔上限 100 MB)。影片請用連結(YouTube/Instagram/Facebook/Vimeo),不要把影片檔放進 repo。
9. **Driveline 內容有版權**:只整理課表結構、翻譯簡短說明,保留出處,影片連到官方原始連結。不要大段複製原文。
10. **Google 表單**:`build.py` 依照題目標題的關鍵字對應網頁欄位(見 `FORM_FIELDS`)。表單題目改名後要確認關鍵字仍對得上。

## 完成前的檢查

1. 執行 `python3 tools/build.py`,確認沒有新的錯誤,「資料檢查」沒有因你的修改而多出提醒。
2. 用瀏覽器開預覽,實際操作改到的功能,並確認 Console 沒有錯誤。要檢查手機寬度和深色模式。
3. commit 訊息用一句話說明「為什麼改」;如果是處理某個 Issue,寫上 `fixes #編號`,合併後 Issue 會自動關閉。

## template.html 程式碼地圖

JS 依區塊排列,每段開頭有 `// ---------- 名稱 ----------` 註解:

| 區塊 | 內容 |
|---|---|
| 開頭 | `DATA`(build.py 注入的資料)、`esc()`、`store`(localStorage 包裝)、項目索引 `byId` |
| 影片 | `videoInfo()` 判斷網址類型,`videoBlock()` 產生播放器或外部連結 |
| 共用 | 分頁定義 `TABS`、`chip()`、`pill()`、`paragraphs()` |
| 項目庫 | 篩選狀態 `itemState`、依部位分組的列表 |
| 項目詳細 | 底部彈出的詳細頁 `openSheet()` |
| 隊內菜單 | `renderMenus()` |
| Driveline | 週期課表、課表一覽、器材 |
| 知識 | Driveline 說明文章 |
| 回饋・新增 | 送到 Google 表單 `submitForm()` |
| 路由 | hash 路由:`#/items`、`#/item/<id>`、`#/menus`、`#/dl`、`#/learn`、`#/add` |
