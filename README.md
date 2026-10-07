# 富士食譜工具與拍立得小房間

把富士相機食譜做成可瀏覽、可收藏的小工具，並附一套本機即可使用的拍立得風格邊框工作室。  
兩個入口互相連通，不必安裝、不必建置，用瀏覽器直接開 HTML 就能用。

| 入口 | 檔案 | 用途 |
| --- | --- | --- |
| 富士食譜工具 | `index.html` | 瀏覽、搜尋、收藏相機食譜 |
| 拍立得小房間 | `polaroid.html` | 匯入照片、套邊框、寫相機資訊、單張／批次輸出 |

線上示範（若已部署 GitHub Pages）：

- https://enweilo.github.io/fuji-recipes-polaroid/

---

## 快速開始

1. 保留專案相對路徑，至少要有：
   - `index.html`、`polaroid.html`
   - `fonts/`、`assets/`、`vendor/`（若有）
   - `manifest.webmanifest`、`icon-192.png`、`icon-512.png`、`sw.js`
2. 用瀏覽器直接開啟 `index.html` 或 `polaroid.html`（`file://` 可用）。
3. 若要用 PWA／離線快取，請用本機 HTTP 伺服器開啟，例如：

```bash
npx --yes serve .
```

然後連到提示的網址。`file://` 不會註冊 Service Worker。

兩頁頂部都有互相跳轉的連結。

---

## 拍立得小房間

`polaroid.html` 即獨立專案〈拍立得小房間〉（[EnWeiLo/polaroid-tool](https://github.com/EnWeiLo/polaroid-tool)）的 `index.html`，在這裡改名為 `polaroid.html`，頂部另加一顆「富士食譜工具」返回連結。

把照片做成拍立得／社群比例輸出，每張照片各自記住裁切、文字與來源設定。

### 功能

- 一次最多 **20 張**。
- 邊框：白框、黑框、取色、復古、底片。
- 比例：原圖、Mini、方框、Wide、橫幅、Instagram 貼文（4:5）、Instagram 限時動態（1080 × 1920）。
- 拖曳裁切，雙指或滾輪縮放。
- 單張下載／分享，多張打包 ZIP。
- 首次開啟有導覽，右上角「?」可重看。

### 主題

右上角可切換：工作桌、太空、日式、中式、像素、科幻、OLED 全黑（預設）。選擇記在瀏覽器裡。

### EXIF 與顯示資料

匯入時會讀取相機資料並自動帶入文字欄位，品牌、型號、鏡頭、曝光等可各自改成手動輸入或不顯示。

**不會改寫照片檔案裡的 EXIF。**

手機辨識走可擴充的 `PHONE_RULES`；認不出來的裝置走一般相機流程。  
目前 EXIF 解析以 JPEG 為主。沒有 EXIF、或不支援的容器／機型，自動帶入資料會不完整，可改手動輸入。

### 字體

本地字體由 `fonts/` 載入：

- jf open 粉圓（`jf-openhuninn-2.1.ttf`）
- 辰宇落雁（`ChenYuluoyan-2.0-Thin.ttf`）
- 源流明體（`GenRyuMin2TW-*.otf`）

其餘字體走 Google Fonts，**需要網路**。`fonts/` 內另有 `-EL`、`-L`、`-R` 字重，目前拍立得小房間未使用，保留給舊版備份／日後使用。

### 機型圖示

Z5 II、Z6、X-T50 的向量圖示已內嵌在 `polaroid.html`，避免 `file://` 載不到外部圖而讓 Canvas 匯出失敗。`assets/models/` 內是同一批 SVG 原檔。

---

## 富士食譜工具

`index.html` 是食譜瀏覽端：搜尋、卡片、收藏與行動裝置版面。  
可安裝成 PWA（`manifest.webmanifest` + 圖示）。部分功能若接了雲端後端，需要網路。

兩頁視覺語彙一致：同樣的 7 個主題（工作桌、太空、日式、中式、像素、科幻、OLED 全黑）與流體玻璃質感（半透明面板、邊緣高光、會緩慢飄移的背景光斑）。  
主題選擇存在同一個瀏覽器設定裡，在任一頁切換，另一頁也會跟著變。預設是 OLED 全黑。

食譜卡片在桌面寬度會排成兩欄；手機上為了捲動順暢，卡片不做背景模糊，只保留半透明與邊緣高光。

---

## 目錄說明

```
.
├── index.html              食譜工具
├── polaroid.html           拍立得小房間
├── manifest.webmanifest
├── sw.js                   僅清理舊快取，不攔截請求
├── icon-192.png
├── icon-512.png
├── fonts/                  本地中文字型
├── assets/models/          機型 SVG
├── vendor/                 第三方函式庫（若有）
├── qa/                     驗證腳本、截圖、匯出樣本
├── backups/                改版前快照
└── original/               最初匯入的原始檔，請勿當工作複本改
```

移動整個資料夾時，請維持上述相對位置。

---

## 快取、PWA、備份

- `sw.js` **不**攔截資源。在 HTTP 環境更新時，只清掉 `fuji-recipe-v*` 舊快取，讓兩個入口吃到最新檔。
- `file://` 不註冊 Service Worker。
- 改版前的入口、README、manifest、SW 放在 `backups/2026-09-10-before-update/`。
- `original/` 與最初 ZIP 內容未改。

若要還原該次備份：先另存目前版本，再把備份目錄裡的五個檔案複製回專案根目錄。這會把「哪個檔是主入口」一併還原成修改前狀態。

---

## 驗證

這是直接跑的 HTML / Canvas 專案，沒有正式 build、lint 或單元測試套件。  
回歸檢查在 `qa/`（這批腳本與截圖是針對替換前的舊版 `polaroid.html` 寫的，換成〈拍立得小房間〉後部分選擇器與預期畫面已不適用，需要時請依新版調整）：

| 腳本 / 產物 | 內容 |
| --- | --- |
| `qa/verify.cjs` | 真實 JPEG EXIF、手機＋相機混合、來源選單、舊字體 fallback、字體尺寸表、選色效能、響應式、雙向導航 |
| `qa/verify-exports.cjs` | 透明圖示、PNG／ZIP 下載與 CRC、批次輸出後還原、SW 快取範圍 |
| `qa/color-before.json`、`qa/verification.json`、`qa/export-verification.json` | 實測結果 |
| `qa/*fields.png`、`qa/font-size-sheet.png` 等 | 視覺對照 |

本機需已安裝 Node.js：

```bash
node qa/verify.cjs
node qa/verify-exports.cjs
```

測試使用本機 Edge 與 Playwright。可用環境變數指定路徑：

- `PLAYWRIGHT_MODULE`
- `STUDIO_ROOT`

這些套件只給驗證用，網站本身不依賴它們。

---

## 字型授權

`fonts/` 內檔案各有原授權（例如源流明體的 `GenRyu-OFL.txt`、粉圓／落雁的授權條款）。  
再散布專案時請一併保留授權檔，並遵守各字型的 OFL／原作者條件。

---

## 注意事項

- 建議用較新的 Chromium / Edge / Safari；Canvas 匯出與字型載入在舊瀏覽器可能有差。
- 批次匯出張數多、解析度高時，記憶體用量會明顯上升，建議分批處理。
- 非 JPEG、或被社群 App 洗掉 EXIF 的圖，自動帶入資料會不完整，可改手動輸入。
- 本 README 的拍立得小房間段落對應 2026-10 替換為〈拍立得小房間〉之後的狀態；食譜工具與其餘段落沿用 2026-09-10 快照。
