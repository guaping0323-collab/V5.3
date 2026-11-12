# Patch 1 測試 ID 與重複口徑對照

## 來源與唯一識別

- **原始 41 項**：保留原有功能 ID（DB-01～DB-15、GAME-01～GAME-12、INTENT-01～INTENT-14），以 `test_results/original_41/test_results.json` 中的 ID 作為唯一鍵。
- **Phase 4 擴充 49 項**：以 `test_phase4_p0.py`、`test_growth_v2.py`、`test_character_state_v5.py` 的完整 `ClassName.test_method` 作為唯一鍵。套件組成 18 + 23 + 8。
- **Patch 1 新增測試**：以 `test_patch1_engineering.py` 的完整 `ClassName.test_method` 作為唯一鍵，共 23 項。
- 三種 ID 是不同測試來源，不代表互斥需求；原始功能 ID 與單元測試可能覆蓋相同行為，不能相加作為獨立需求總數。

## 主要重疊對照

| 原始功能 ID | Phase 4 擴充測試 | Patch 1 新增測試 | 重疊說明 |
|---|---|---|---|
| DB-01～DB-05 | `test_growth_v2.py` 中 `test_real_desktop_startup_validates_v3_database`、`test_main_program_prompt_and_worker_wiring` | `DatabasePathTests` 全部 | 路徑解析與缺檔 fail-closed；Patch 1 新增舊 SOL 入口契約測試 |
| DB-06～DB-08 | `test_growth_v2.py` 中 `test_read_operations_do_not_modify_database_bytes`、`test_real_post_answer_extractor_saves_only_evidence_backed_item` | `MemoryLifecycleTests`、`MigrationSafetyTests` | 都使用 disposable DB；Patch 1 擴大短期記憶 CRUD / WAL 覆蓋 |
| DB-09～DB-15 | `test_growth_v2.py` 中 `test_game_classification_schema_uses_non_overwriting_backup`、Migration 相關測試 | `MigrationSafetyTests` 全部 | 備份、rollback、WAL 與失敗注入有重疊；新增測試加強 Migration API 自帶備份 |
| GAME-07 | `ClassificationTests.test_actual_updater_entry_point_blocks_unapproved_before_search` | 無需重寫；另有搜尋故障隔離測試 | 既有動態 spy 保留，未核准候選外部呼叫為 0 |
| GAME-10～GAME-12 | `ClassificationTests` 身份測試；`test_identity_scoped_web_research_keeps_only_structured_exact_match` | `ClassificationIdentityTests` | canonical ID、alias、server/region 同一行為多層覆蓋 |
| INTENT-04/06/09 | `IntentRoutingTests` 的 history/companionship/context 測試 | `IntentRegressionTests.test_context_chat_does_not_route_but_explicit_search_does` | 同一類誤觸搜尋回歸，分別由原始 ID、Phase 4 單元與 Patch 1 測試覆蓋 |
| INTENT-14 | `test_web_context_labels_external_content_untrusted` 及 redirect 測試 | `URLSecurityTests` | 只覆蓋 URL/不可信內容處理；OS/container sandbox 仍 NOT_TESTED/BLOCKED |
| Growth 18 項舊報告 | `test_growth_v2.py` 23 個方法 | `MemoryLifecycleTests`、Migration tests | 18 是舊報告的需求分組；目前 23 個方法將其中整合測試拆分並補充，不能另加 18 |
| 角色素材 8 項 | `test_character_state_v5.py` 8 項 | 無新增素材測試 | 8 項已包含在擴充 49 中，不可重複加總 |

## 統計規則

- 原始 41 ID 本次重跑：40 PASS、0 FAIL、0 PARTIAL、1 NOT_TESTED。
- Phase 4 擴充單元測試：49 PASS。
- Patch 1 新增單元測試：23 PASS。
- 49 + 23 = 72 次單元測試方法執行；方法間仍有需求覆蓋重疊。
- 原始 41 ID 是另一套驗收 ID，與單元測試重疊，故不計入 72，也不與 49/23 相加。