# V5.3 安裝與啟動說明

## 1. 系統需求

- Windows 10／11（64-bit）
- Python 3.13（建議使用官方 Python Launcher `py`）
- Ollama
- Ollama 模型：`qwen3:8b`
- 網路搜尋套件由 `requirements.txt` 安裝；搜尋服務仍可能因網路或供應商狀態不可用。

## 2. 安裝

1. 將候選 ZIP 解壓縮到可讀寫的獨立資料夾。
2. 確認三份原有 SQLite DB 位於專案目錄：`sol_history.db`、`yae_game_database.db`、`八重神子成長資料.db`。
3. 雙擊 `INSTALL_V5.bat`，建立 `.venv` 並安裝依賴。
4. 安裝 Ollama，啟動 Ollama，執行：

```powershell
ollama pull qwen3:8b
```

5. 雙擊 `launcher.bat` 啟動桌寵。`START_V5.bat` 與 `START_4號版本.bat` 也保留作相容啟動入口。

## 3. 分類資料庫啟用（選用）

遊戲情報搜尋採 fail-closed。若尚未建立分類資料表，所有候選都不會進入自動情報搜尋。這是刻意的安全預設。

如要啟用分類持久化：

1. 完全關閉桌面程式，確認沒有其他程式連線遊戲 DB。
2. 在專案目錄執行：

```powershell
.venv\Scripts\python.exe -c "from desktop_pet import GameDatabase; print(GameDatabase.initialize_classification_schema())"
```

3. 程式會先建立 `yae_game_database.db.pre_classification.bak`；若備份已存在，會拒絕覆蓋並停止。
4. 重新啟動程式並執行遊戲掃描。Steam manifest 候選會依來源證據分類；其他平台候選通常保留待覆核狀態。
5. 人工分類 API 預設 `dry_run=True`。檢查結果後，才可明確使用 `dry_run=False` 寫入。現階段尚無完整 GUI 覆核面板。

## 4. 資料庫安全

- 啟動時只驗證既有 DB，不會自動建立空白正式 DB。
- Growth System Migration 不會由桌面程式自動執行。
- 分類資料表初始化必須明確呼叫，且先建立備份。
- 不要在桌面程式執行時手動複製／覆寫 DB。
- 如需自訂 DB 路徑，使用 `YAE_SOL_DB`、`YAE_GAME_DB`、`YAE_GROWTH_DB` 或 `YAE_DATABASE_DIR`；所有路徑必須指向既有檔案。

## 5. 測試與限制

- `python -m unittest -v test_phase4_p0 test_growth_v2 test_character_state_v5` 可執行非 GUI 測試；角色素材測試需要 Pillow，可參考 `requirements-test.txt`。
- `test_windows_gui_v5.py` 必須在 Windows + PySide6 桌面環境執行。
- Ollama 真實模型、Windows Registry／Xbox／Epic 掃描、桌面滑鼠事件及正式 DB 寫入不由 Linux 測試替代。
- 外部網頁內容不是指令，但目前沒有 OS／容器級沙箱；不要將此版本視為具備完整安全沙箱。