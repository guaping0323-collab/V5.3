# V5.4 Backlog（Patch 1 不執行）

1. **DNS pinning / rebinding protection**：目前逐跳 URL 驗證及 DNS 公網位址檢查仍存在「驗證時解析」與「HTTP 連線時再次解析」的 TOCTOU 間隙；需以可驗證的 IP pinning transport 或隔離代理處理。
2. **OS / container network sandbox**：以作業系統或容器網路政策限制外部內容可存取的網段、檔案及程序；Python URL 檢查不能取代此邊界。
3. **Worker lifecycle completion**：統一管理 `YaeAnswerWorker`、`YaePostAnswerWorker`、手動掃描 worker 的取消、等待及關閉狀態；禁止以強制終止執行緒作一般關閉方式。
4. **Windows Qt lifecycle**：在 Windows 實機驗證 QTimer parent/stop/release、Signal 回 GUI thread、視窗關閉及多次啟停。
5. **Real Ollama end-to-end**：驗證 qwen3:8b 連線中斷、逾時、恢復、回答及記憶整理。
6. **Real DDGS HTTP**：驗證實際套件版本、HTTP 逾時、網路中斷及恢復；目前只做 mock failure-path。
7. **Game classification GUI**：提供人工覆核面板及分類變更證據檢視。
8. **Server / region natural-language parser**：解析前先要求使用者確認模糊的伺服器與區域，不得自行猜測。
9. **Structured logging coverage**：將剩餘 print/exception 訊息逐步改為安全事件日誌，並驗證輪替、權限與敏感資料遮罩。
10. **Migration crash-injection**：以子程序 kill/crash 測試驗證每個 Migration 階段的程序中斷恢復；Patch 1 僅完成 transaction fault injection、寫入失敗及 WAL 測試。
11. **Resource monitoring**：完整記錄長期 CPU/RAM/執行緒/Timer 狀態。
12. **Multi-AI collaboration / new character animation / architecture refactor**：僅列待辦，不納入本次補丁。