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

> 需要先被加入協作者(Write 權限)。被加入的人修改後**不需要審核**,推送後約 1–2 分鐘網站就會更新,所以改之前請先在自己電腦上預覽確認。

> 用 AI 工具(Claude Code、Cursor、Copilot 等)幫忙寫程式的話,AI 會自動讀取 [AGENTS.md](AGENTS.md) 裡的專案規則。要做什麼可以到 **Issues** 分頁找,做完在 commit 訊息寫 `fixes #編號`,Issue 會自動關閉。

#### 先知道要改哪個檔案

| 想改的東西 | 改這個檔案 |
|---|---|
| 網頁的樣子、文字、按鈕、新功能 | `tools/template.html`(HTML、CSS、JavaScript 都在這一個檔案) |
| 讀取 Excel 的方式、資料檢查規則、Google 表單對應 | `tools/build.py` |
| 訓練資料本身 | 不用改程式,改 `棒球訓練2026_整合.xlsx`(見上一節) |

**不要改 `index.html`**。它是 `build.py` 用 Excel 和 `template.html` 自動產生的,手動改了也會在下次產生時被蓋掉,而且它不在 repo 裡。

#### 方法一:直接在 GitHub 網頁上改(適合改幾個字)

1. 在 repo 點進 `tools/template.html`
2. 按右上角的鉛筆圖示(Edit this file)
3. 修改後按綠色的 **Commit changes**,寫一句說明改了什麼,再按一次 **Commit changes**
4. 到 **Actions** 分頁,等最新一筆出現綠色勾勾,打開網站確認

這個方法無法先預覽,改錯會直接影響網站。只適合改錯字這類小地方。

#### 方法二:在自己電腦上改(建議)

**第一次準備**(只做一次):

1. 安裝 [Git](https://git-scm.com/downloads) 和 [Python 3](https://www.python.org/downloads/)(Mac 通常已內建 Git)
2. 打開終端機(Mac 的「終端機」;Windows 的「PowerShell」),逐行執行:
   ```bash
   git clone https://github.com/kobechiu/ntugbbaseball.git
   cd ntugbbaseball
   pip3 install openpyxl
   ```
   完成後,電腦上會多一個 `ntugbbaseball` 資料夾,裡面就是整個專案。

**每次修改的流程**:

1. **下載最新版**(別人可能改過,一定要先做):
   ```bash
   cd ntugbbaseball
   git pull
   ```
2. **用任何文字編輯器修改檔案**(推薦 [VS Code](https://code.visualstudio.com/))
3. **在自己電腦上預覽**:
   ```bash
   python3 tools/build.py
   python3 -m http.server 8000
   ```
   用瀏覽器打開 <http://localhost:8000>,確認修改正確。記得用手機尺寸檢查(Chrome 按 F12 → 左上角手機圖示),大部分隊友是用手機看。看完回到終端機按 `Ctrl + C` 關閉預覽。
4. **上傳到 GitHub**:
   ```bash
   git add .
   git commit -m "用一句話說明改了什麼"
   git push
   ```
   第一次 push 如果要求登入,依畫面指示登入 GitHub 帳號(或先執行 `gh auth login`)。
5. 到 repo 的 **Actions** 分頁,等最新一筆出現綠色勾勾,打開網站確認。

#### 改壞了怎麼辦

- **Actions 出現紅色叉叉**:點進去看錯誤訊息。這時網站還是上一個正常版本,修正後再 push 一次即可。
- **網站打得開但壞掉了**:到 repo 首頁點 **commits**(時鐘圖示),找到出問題的那一筆,複製右邊的 7 碼編號(例如 `96966e2`),然後在終端機執行:
  ```bash
  git pull
  git revert --no-edit 96966e2
  git push
  ```
  (把 `96966e2` 換成你複製的編號。)這會新增一筆「取消那次修改」的紀錄,約 1–2 分鐘後網站恢復。不會用終端機的話,請找會的隊友幫忙。
- **`git push` 被拒絕**(訊息有 `rejected`):代表別人在你之後推送過,先執行 `git pull` 再 `git push`。

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
