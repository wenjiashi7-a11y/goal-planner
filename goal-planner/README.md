# 理財目標導航

以理財目標為核心的 PV / FV / PMT 試算網站：設定目標 → 診斷達成率 → 取得行動建議。
純靜態網站，不需建置步驟、不需後端或資料庫。

## 功能
- **目標分析**：退休、子女教育、購屋、緊急預備金、自訂目標；計算通膨後目標終值、預估終值、達標所需月投入（PMT）與報酬率，並附 Excel 公式
- **資產累積路徑圖**、情境壓力測試（報酬率 ±2%）、逐年明細表
- **行動建議**：提高 PMT、延長年期、一次補足 PV、報酬率需求與風險提醒
- **PV · FV · PMT 工具箱**：三個獨立計算器，PMT 支援「存錢達標」與「貸款攤還」
- **全部目標總覽**：多目標每月現金流檢查
- 使用者資料只存在各自瀏覽器的 localStorage，不會上傳

## 專案結構
```
goal-planner/
├── public/                 ← 網站本體（Cloudflare 部署這個資料夾）
│   ├── index.html
│   ├── favicon.svg
│   ├── robots.txt
│   ├── _headers            ← Cloudflare Pages 安全與快取標頭
│   └── assets/
│       ├── css/style.css
│       └── js/app.js       ← 試算引擎與畫面邏輯
├── wrangler.toml           ← Cloudflare Pages 設定（輸出目錄 = public）
├── package.json            ← 本機預覽與指令部署（選用）
├── .gitignore
└── README.md
```

## 一、建立 GitHub 儲存庫
**方法 A：網頁上傳（不用指令）**
1. 到 GitHub 按 **New repository**，名稱例如 `goal-planner`，按 Create
2. 在新儲存庫頁面點 **uploading an existing file**
3. 把解壓後 `goal-planner` 資料夾**裡面的所有檔案與資料夾**拖進去（含 `public` 資料夾）→ Commit
   - `.gitignore` 是隱藏檔，若沒拖到也不影響網站運作

**方法 B：指令**
```bash
cd goal-planner
git init
git add .
git commit -m "Initial commit: 理財目標導航"
git branch -M main
git remote add origin https://github.com/<你的帳號>/goal-planner.git
git push -u origin main
```

## 二、Cloudflare Pages 連接 GitHub 自動部署
1. Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. 授權 GitHub，選擇 `goal-planner` 儲存庫 → Begin setup
3. 建置設定：
   | 欄位 | 填入 |
   |---|---|
   | Production branch | `main` |
   | Framework preset | `None` |
   | Build command | （留空） |
   | Build output directory | `public` |
4. **Save and Deploy**，完成後網址為 `https://goal-planner.pages.dev`
5. 之後只要 push 到 GitHub（或在網頁上修改檔案），Cloudflare 會自動重新部署
6. 綁定自訂網域：專案 → **Custom domains** → 例如 `goal.hantutor.com`

## 本機預覽（選用）
直接用瀏覽器開 `public/index.html` 即可；或安裝 Node.js 後執行：
```bash
npm run dev      # 本機預覽 http://localhost:8788
npm run deploy   # 不經 GitHub，直接用 Wrangler 部署
```

## 試算假設
月複利、期末投入，與 Excel `FV(rate/12, n*12, -PMT, -PV)` 一致。報酬率與通膨皆為假設值，本工具供規劃與教學參考，不構成個別投資建議。
