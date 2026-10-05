# 社團器材記錄清理報告與問題清單 (Issues Report)

## 一、處理數量統計
- **原始記錄總列數**：10 列
- **清理後有效列數**：9 列
- **移除無效列數**：1 列（原資料第 6 筆為完全空白之空物件 `{}`，依「整列所有欄位皆為空值才可移除」規則移除）

---

## 二、相同 `item_id` 紀錄分析
依規格要求，相同 `item_id` 之有效列全部保留，不因 ID 重複而合併或刪除：

| item_id | 來源列號 (source_row) | 品名 (name) | 數量 (qty) | 狀態 (status) | 分析與處置 |
|---|---|---|---|---|---|
| `EQ01` | 1, 4 | Marker / 白板筆 | 4 (兩列相同) | available (兩列相同) | **重複紀錄**：所有欄位皆一致。兩列皆保留，待器材管理人確認為重複登載或兩批各4支筆。 |
| `EQ02` | 2, 5 | Extension cord / 延長線 | 2 vs 3 (**衝突**) | borrowed (兩列相同) | **欄位衝突**：第 2 列數量為 2，第 5 列數量為 3。兩列皆保留，不自行猜測數量，待人工核對實際借出數量。 |

---

## 三、數量欄位異常 (Abnormal Quantities)
依規定數量僅接受 0 或正整數，若為空白、負數或其他非有效數值，原樣保留並列入報告，不補 0、不取絕對值、不猜測：

| source_row | item_id | 品名 | 原數量 (qty) | 問題說明與建議 |
|---|---|---|---|---|
| 7 | `EQ04` | Paper pack / 紙張包 | `""` (空字串) | 數量遺失，未填寫數值。保留原始空值，需人工補盤點數量。 |
| 8 | `EQ05` | Tape / 膠帶 | `-1` (負數) | 數量為負值，不合常理。保留原值 `-1`，待確認是盤損、欠品或鍵入錯誤。 |

---

## 四、非標準狀態與未知狀態 (Status Normalization & Unknown)
- **狀態標準化**：
  - `source_row: 1` (`可借`) → `available`
  - `source_row: 3` (` available `) → `available`
  - `source_row: 5` (`借出`) → `borrowed`
  - `source_row: 7` (`可出借`) → `available`
  - `source_row: 10` (`可借`) → `available`
- **未知狀態 (unknown)**：
  - `source_row: 9` (`EQ06` Scissors / 剪刀)：原狀態為 `待盤點`，無法對應至 `available` 或 `borrowed`，依規格標準化為 `unknown`，待盤點後更新狀態。

---

## 五、文字欄位去前後空白
已針對文字欄位（`item_id`, `name`, `status`）清除前後空白字符：
- `source_row: 1`: `" EQ01 "` → `"EQ01"`, `"Marker / 白板筆 "` → `"Marker / 白板筆"`
- `source_row: 3`: `" available "` → `"available"`
- `source_row: 10`: `" EQ07 "` → `"EQ07"`, `" Folder / 資料夾 "` → `"Folder / 資料夾"`
- 注意：`qty` 依規定不套用文字去空白規則，原樣保留型別與數值。
