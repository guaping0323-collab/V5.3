# V5.3 Patch 1 安裝與回退說明

## 安裝前

1. 先關閉八重神子桌面 AI、SOL 舊入口及所有可能使用 SQLite 的程式。
2. 將目前 V5.3 使用資料夾完整備份到另一個位置；不要覆蓋原始 Phase 4 ZIP。
3. 記錄現有三份 DB 的 SHA256。Patch 1 不要求 Migration，也不應在安裝時執行 Migration。
4. 先在新資料夾解壓 `Yae_Miko_Desktop_AI_V5.3_Patch1_Candidate.zip`，不要直接覆蓋舊資料夾。

## 啟動

- Windows 10/11 x64：依既有方式安裝 `requirements.txt` 中的相依套件，並確認 Ollama 與 `qwen3:8b` 已在本機配置。
- 使用 `launcher.bat` 或 `START_V5.bat` 啟動桌寵主程式。
- 若使用 `main.py` / SOL 舊入口，現在會使用相同的 `YAE_SOL_DB` / `YAE_DATABASE_DIR` 路徑規則，且不會在空白檔案中自動建立資料表。
- 三份資料庫可由個別環境變數指定；未指定時使用程式資料夾中的固定檔名。相對路徑以程式資料夾為基準。
- 設定的資料庫路徑不存在時會停止並記錄錯誤，不會建立空白替代資料庫。
- 事件日誌寫入 `logs/v5.3_events.log`，輪替保留最多五份；不記錄完整聊天內容或密鑰。

## Migration

Patch 1 不會自動執行 Growth Migration。若未來確實需要 Migration，必須先確認程式已完全關閉、資料庫位於正確的獨立工作目錄，並由有權限的人員明確執行 `migrate_growth_v2.py`。Migration API 現在會先建立或驗證不可覆蓋的備份；備份失敗即中止。

## 回退

1. 完全關閉桌寵及 SOL 程式。
2. 將啟動路徑切回安裝前的 Phase 4 版本資料夾；優先以整個舊資料夾回切，不要混合新舊 Python 模組。
3. 保留三份 DB 原檔，不要用候選 ZIP 內的 DB 覆蓋使用者目前資料庫。
4. 若曾執行 Migration，使用同一資料庫旁、SHA256 已驗證的 `.bak` 備份回復；不要覆寫既有備份。回復後執行 `PRAGMA integrity_check` 並核對 SHA256。
5. Patch 1 的程式碼回退不需要 Migration，也不需要修改任何 DB。

## 驗收提醒

本交付在 Linux 完成靜態及動態測試，但沒有 Windows、PySide6 GUI 或真實 Ollama 驗收。正式使用前必須依 `WINDOWS_ACCEPTANCE_CHECKLIST.md` 完成 Windows 實機檢查。此候選不代表 RELEASED。