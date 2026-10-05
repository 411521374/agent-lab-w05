# Learning Record / 學習紀錄

- Group code / 組別：G-411521374
- Tool / 工具：Antigravity (Gemini 3.8 Flash)
- Route / 路線：individual 個人
- Tasks completed / 完成題目：A, B, D
- Material / 素材：NDHU classroom tasks 東華課堂版
- For original-pack work: task number, author/source link and version / 原版實作：題號、作者來源連結與版本：無（採用東華課堂版）
- My role and what I checked / 我的角色與實際檢查：負責提示詞輸入、嚴格限定讀寫權限於題目目錄、比對檔案 SHA-256 雜湊確認原檔未遭異動、依測試矩陣執行活動挑選器各條件實測、審查並退回不安全之全域整理計畫。

## Scope and plan / 範圍與計畫

Allowed input and output folders / 可讀取與輸出的資料夾：
- 任務 A：輸入 `practice/01-club-files/input` (唯讀)，輸出 `practice/01-club-files/output` (新增副本與清單)
- 任務 B：輸入 `practice/02-campus-picker/activities.json` (唯讀)，輸出 `practice/02-campus-picker/output/index.html` (單頁應用)
- 任務 D：輸入 `practice/04-review/bad-plan.txt` (唯讀)，輸出 `practice/04-review/my-rejection.md` (退回說明文件)

What I asked for / 原始需求：
- 任務 A：讀取 input 中的 12 個文字檔，不刪除、不修改原檔，依功能分類複製至 output，並產生完整的 `report.md` 與 `manifest.json` 對照清單。
- 任務 B：基於 `activities.json` 製作無依賴之單頁離線 HTML 活動挑選器，支援地點、時間、強度篩選與隨機挑選、保留最近 5 筆紀錄、具重設篩選與中英文雙語支援，並加註教學模擬免責宣告。
- 任務 D：審查 `bad-plan.txt`，挑出至少兩項違規/不安全操作，提出明確退回理由與合理改善方案。

What I checked before execution / 動手前我檢查了什麼：
- 確認 AI 讀寫權限僅限定於當前題目目錄，未授權擴及全磁碟或使用者個人資料夾。
- 盤點輸入檔案數量（共 12 筆文字檔），確認原始檔案未受任何破壞。
- 審查 AI 提出的第一階段執行計畫，確認不會擅自刪除檔案、覆蓋現有內容或依檔名妄斷定稿。

## Tests actually performed / 我真的做過的測試

| Test / 測試 | Expected / 預期 | Observed / 實際 | Evidence / 證據 |
|---|---|---|---|
| 1. 室外／15分鐘／中強度 | 顯示「沒有符合條件的活動」，不放寬條件 | 顯示「沒有符合條件的活動，請調整篩選。」（英文：No matching activity.），活動池顯示 0 項 | 網頁結果區塊顯示無相符提示，未隨機選出任何不符條件之活動 |
| 2. 室外／30分鐘／中強度 | 每次都只能選到 A09（在合適位置快走） | 連續點擊「幫我選」，每次選出的活動 ID 皆為 A09，耗時 30 分鐘，強度中 | 結果區顯示 A09、在合適位置快走、30分鐘、室外、中 |
| 3. 重設篩選測試 | 條件回到不限地點、30分鐘、不限強度，歷史紀錄保留 | 下拉選單分別回到「不限」、「30」、「不限」，歷史清單（Recent picks）5 筆紀錄完好保留 | 檢視控制項數值與歷史列表狀態均符合預期 |

## One revision / 一次修改

Before / 原來的情況：
在 B v1 第一版中，點擊「重設篩選」按鈕時，可用時間選單被設為「不限」（any），未依規格回到「30分鐘」；且介面缺少規格要求的「教學模擬活動，不是校方公告」聲明字樣。

Request / 我提出的修改：
1. 修正重設篩選事件處理函數，將時間預設值指派為 "30"。
2. 在頁尾增加 `<footer class="disclaimer">` 並新增中英文多國語言字串（"教學模擬活動，不是校方公告" / "Teaching simulation activity, not an official campus announcement"）。

After and retest / 修改後與重測結果：
點擊「重設篩選」後，時間選單正確顯示「30分鐘」，符合條件之活動數量即時顯示為 7 項；頁面底部正確呈現模擬宣告文字，切換語言亦可同步翻譯。

New requirement or defect? / 新需求還是原規格未做到？
原規格未做到（defect）。原始規格第 8 點明確載明「回到不限地點、30分鐘、不限強度」，且頁面規格要求「標明『教學模擬活動，不是校方公告』」。

## One rejection / 一次退回

Which action I reject and why / 退回哪個動作、為什麼：
退回 `bad-plan.txt` 中的以下危險動作：
1. 擅自將處理範圍擴大至整個 `Downloads` 目錄（超出授權目錄，可能破壞或洩漏個人檔案）。
2. 自行刪除重複檔案，並單憑檔名 `final2` 認定為最新版本（缺乏實質核准依據，可能刪除關鍵草案）。
3. 缺失資料時自行填補猜測數值（造成虛假資料誤導後續決策）。
4. 整理完成後自動公開發布（未經人工確認發布，嚴重違反資安防護）。

An acceptable alternative / 可以怎麼改：
- 嚴格限定在指定之專案目錄內執行，不跨出邊界。
- 完整保留所有檔案副本，透過 `report.md` 與清單標示疑似重複檔案，不逕行刪除。
- 遇未確定數值標註為 `TBD` 或保留空白並列入待確認報告，不擅自揣測。
- 輸出成果留待人工核閱，絕不執行任何自動上傳或公開發布動作。

## Still unverified / 還沒驗證

What I cannot claim is complete / 哪些事不能說已完成：
- 任務 A 兩份提案（室外 30 分鐘 vs 室內 20 分鐘）與未核准之紙張預算，需待社團幹部真實開會方能正式定案。
- 任務 B 活動挑選器的隨機分佈在數學大樣本下的均勻性未進行嚴謹卡方檢定，目前僅驗證介面互動與篩選邏輯之正確性。

For the fallback route, mark all prepared evidence as supplied simulation. / 備援路線請標明所有預生成證據來源，不能填成自己的Agent實跑。
- 本次實作為個人 Agent 實際執行，非備援路線。
