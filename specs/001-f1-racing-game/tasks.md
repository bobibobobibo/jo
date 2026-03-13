# Tasks: F1 賽車遊戲

**Input**: Design documents from `/specs/001-f1-racing-game/`  
**Prerequisites**: plan.md ✓, spec.md ✓

**Tests**: TDD 採手動驗收場景（spec.md 中已定義），每個 User Story 完成後逐條驗證。

---

## Phase 1: Setup（共用基礎建設）

**目的**: 專案結構初始化

- [x] T001 確認單一靜態網頁交付策略（`index.html` at repo root）
- [x] T002 確認 GitHub Pages 部署無需額外設定（`main` branch root）

---

## Phase 2: Foundational（核心基礎）

**目的**: 必須在任何 User Story 實作前完成的核心基礎

- [x] T010 定義常數（CANVAS_W/H、TOTAL_LAPS、STORAGE_KEY、STATE enum）
- [x] T011 實作工具函數（fmtTime、rrPath、segsCross、closestOnSeg）
- [x] T012 建立 Canvas 元素與自適應縮放（resizeCanvas）
- [x] T013 建立鍵盤輸入處理（keydown/keyup WASD + 方向鍵 + ESC）
- [x] T014 建立 visibilitychange 失焦自動暫停

**Checkpoint**: 基礎工具就緒，可開始 User Story 實作

---

## Phase 3: User Story 1 — 駕駛 F1 賽車完成比賽（Priority: P1）🎯 MVP

**目標**: 可玩的賽車主迴圈——賽車 + 賽道 + 圈數計時 + 完賽

**獨立測試**: 開啟遊戲 → 點按開始 → 跑完 3 圈 → 看到完賽畫面

### US1 驗收測試（TDD 紅燈確認）

- [x] T020 [US1] 手動執行 AC1：開啟頁面確認無賽道（紅燈 → 實作後綠燈）
- [x] T021 [US1] 手動執行 AC2：確認無鍵盤控制（紅燈 → 實作後綠燈）
- [x] T022 [US1] 手動執行 AC3：確認無完賽畫面（紅燈 → 實作後綠燈）

### US1 實作

- [x] T023 [US1] 定義賽道資料結構（Track class：控制點、寬度、終點線）
- [x] T024 [US1] 實作 Monza 橢圓賽道（60 點橢圓插值、width=72）
- [x] T025 [US1] 實作 Monaco 技術賽道（15 手工控制點、width=62）
- [x] T026 [US1] 實作 Track.draw()（Midpoint Bezier 路徑、格紋終點線）
- [x] T027 [US1] 實作 Track.constrainCar()（邊界碰撞、速度懲罰）
- [x] T028 [US1] 定義賽車資料（Car class：位置、角度、速度、物理常數）
- [x] T029 [US1] 實作 Car.update()（加速、煞車、摩擦、速度感應轉向）
- [x] T030 [US1] 實作 Car.draw()（F1 頂視圖：車體、前後翼、駕駛艙、輪胎）
- [x] T031 [US1] 實作 Race class（圈數追蹤、終點線偵測 segsCross、方向防偽）
- [x] T032 [US1] 實作 Game Loop（startLoop/stopLoop/loop，dt 正規化）
- [x] T033 [US1] 實作 render()（草地背景 + 賽道 + 賽車）
- [x] T034 [US1] 實作倒數計時（3→2→1→GO! setTimeout 鏈）
- [x] T035 [US1] 實作 endRace() → 顯示完賽畫面
- [x] T036 [US1] Git checkpoint（`git status`、驗收場景 AC1/AC2/AC3 通過、commit）

**Checkpoint**: US1 可獨立完整遊玩

---

## Phase 4: User Story 2 — 查看成績與最佳圈速（Priority: P2）

**目標**: 完賽後詳細成績 + localStorage 歷史最佳圈速

**獨立測試**: 完賽 → 看到每圈圈速與總用時 → 重玩 → 確認最佳圈速更新

### US2 驗收測試

- [x] T040 [US2] 手動執行 AC1：完賽畫面顯示每圈圈速與總用時（紅燈）
- [x] T041 [US2] 手動執行 AC2：歷史最佳圈速顯示與更新（紅燈）
- [x] T042 [US2] 手動執行 AC3：首次遊玩自動建立基準（紅燈）

### US2 實作

- [x] T043 [US2] 實作 localStorage helpers（getRecords、saveRecord、getRecord）
- [x] T044 [US2] 實作完賽畫面（每圈圈速列表、最快圈標示 ★、總用時）
- [x] T045 [US2] 實作新紀錄徽章（🏆 新圈速紀錄！）
- [x] T046 [US2] 實作 HUD（目前圈速計時、最佳圈速、速度、圈數）
- [x] T047 [US2] 實作暫停功能（ESC toggle、pause-overlay）
- [x] T048 [US2] Git checkpoint（驗收場景 AC1/AC2/AC3 通過、commit）

**Checkpoint**: US1 + US2 均可獨立運作

---

## Phase 5: User Story 3 — 選擇賽車與賽道（Priority: P3）

**目標**: 選車（3 款）+ 選道（2 條）流程

**獨立測試**: 選不同車/道組合 → 開始比賽 → 確認外觀與賽道一致

### US3 驗收測試

- [x] T050 [US3] 手動執行 AC1：3 款賽車可選（紅燈）
- [x] T051 [US3] 手動執行 AC2：2 條賽道可選（紅燈）
- [x] T052 [US3] 手動執行 AC3：選擇與比賽內容一致（紅燈）

### US3 實作

- [x] T053 [US3] 定義 CAR_DEFS[]（Red Bull / Mercedes / Ferrari，含配色）
- [x] T054 [US3] 實作選車畫面（drawMiniCar、選中高亮、onclick 更新）
- [x] T055 [US3] 定義 TRACK_DEFS[]（Monza / Monaco，含描述）
- [x] T056 [US3] 實作選道畫面（drawTrackPreview、最佳圈速顯示）
- [x] T057 [US3] 實作主選單 → 選車 → 選道 → 比賽完整流程
- [x] T058 [US3] Git checkpoint（驗收場景 AC1/AC2/AC3 通過、commit）

**Checkpoint**: 所有 User Story 均可獨立運作

---

## Phase 6: Polish & Cross-Cutting

**目的**: 跨 User Story 品質提升

- [x] T060 CSS 美化（F1 主題配色、響應式縮放、HUD 半透明）
- [x] T061 邊緣案例驗證（邊界碰撞、倒退防假圈、localStorage 失敗靜默跳過）
- [x] T062 SC-001~006 成功標準手動驗證
- [x] T063 最終 git checkpoint（所有驗收場景通過、PR 推送）

---

## Dependencies & Execution Order

- **Phase 1–2（Setup + Foundational）**: 無依賴，立即開始
- **Phase 3（US1）**: 依賴 Phase 2，最高優先
- **Phase 4（US2）**: 依賴 Phase 3（需要 Race 完賽資料）
- **Phase 5（US3）**: 依賴 Phase 2，可與 Phase 4 並行
- **Phase 6（Polish）**: 依賴所有 User Story 完成

## Notes

- 所有任務均在單一 `index.html` 中完成，無需構建步驟
- 勿刪除或覆寫 `spec.md`、`plan.md`、`tasks.md`
- 每個 Task 完成即勾選 `[x]`
- GitHub Pages 交付：repo root 的 `index.html` 直接可用
