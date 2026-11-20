# amazon-ppc-advisor — Amazon PPC 關鍵字廣告建議工具

單檔前端工具（`index.html`＋`manual.html`），2026-09-21 建立。輸入商品售價、目標 ACOS、預估轉換率 → 算出 Max CPC；關鍵字依熱度分競爭度給出價區間，並規則式分成精準／詞組／廣泛三種 Amazon 廣告活動，另抓出否定關鍵字候選。

## 分工原則（改動前務必理解）
- **出價數字全部由前端公式計算，不需 API 金鑰**。選用 BYOK AI（Claude／OpenAI／Gemini／OpenRouter）只補文字策略建議；prompt 明確禁止 AI 產生金額，前端也不會用 AI 的數字覆蓋計算結果。
- 可匯入 `product-keyword-cloud`（跨境電商關鍵字文字雲）匯出的 CSV，它的四分類標籤直接對應比對類型。
- Gemini 呼叫已套 `geminiModelFix()`（2026-11-09），Google 再下架模型時更新對照表即可。

## 序號授權
- 全螢幕 `#licenseGate`，骨架沿用 `product-keyword-cloud`。後端是綁定共用試算表「**跨境電商工具**」的 Apps Script，獨立分頁「PPC序號」，12 個月期限。
- 注意與「跨境電商產品標題」試算表名稱相近、容易搞混（見全域記憶 `google-sheets-similar-names-confusion`）。
- `clasp create --parentId` 會把 `appsscript.json` 重置成預設值，推送前要改回 `Asia/Taipei`＋`executeAs: USER_DEPLOYING`／`access: ANYONE_ANONYMOUS`。首次 OAuth 授權必須由使用者本人在 Apps Script 編輯器完成。
- 測試 POST 用 Node `fetch()`，不要用 curl（302 轉址會被降級成 GET）。

## 部署
公開 repo，GitHub Actions（`.github/workflows/deploy-pages.yml`）部署 Pages：<https://m255525.github.io/amazon-ppc-advisor/>。push 到 master 自動部署。

## 標準配備
跑馬燈（滑鼠移入暫停）、`manual.html`（含完整創作者區塊）、使用警語、PWA（`service-worker.js` 只刪自己前綴的舊快取）、訪客計數器。改了快取清單內的檔案記得升 `CACHE_NAME`。

## 驗證
無自動化測試；改動後用工具健康檢查（`資料儀表板/tool-health-dashboard`）重查，並以瀏覽器實際開頁確認序號閘門與計算。
