# Feature Specification: 任務管理

**Feature Branch**: `002-task-management`  
**Created**: 2026-03-13  
**Status**: Draft  
**Input**: User description: "tasks"  
**Language**: 繁體中文（敘述文字強制；程式碼與專有名詞可保留原文）

## User Scenarios & Testing *(mandatory)*

### User Story 1 - 新增與完成任務 (Priority: P1)

使用者在瀏覽器中開啟任務清單頁面，輸入任務標題後新增一筆任務，清單隨即顯示該任務。使用者可勾選任務將其標記為「已完成」，已完成的任務以視覺方式（如刪除線或灰階）區分於待辦任務。

**Why this priority**: 新增與完成任務是任務管理的最核心操作——沒有這個功能，整個功能無法成立。可獨立交付為最小可行版本（MVP）。

**Independent Test**: 可透過開啟主頁、輸入任務標題並送出、再勾選完成來完整驗證；若任務出現在清單且勾選後視覺狀態改變即通過測試。

**Acceptance Scenarios**:

1. **Given** 使用者開啟任務清單頁面，**When** 使用者在輸入框輸入任務標題並按下新增，**Then** 新任務立即出現在清單底部，輸入框清空並可繼續輸入。
2. **Given** 清單中存在一筆待辦任務，**When** 使用者勾選該任務的核取方塊，**Then** 任務標題呈現刪除線樣式，並歸類至已完成狀態。
3. **Given** 使用者嘗試新增空白標題的任務，**When** 使用者在輸入框未輸入任何文字即按下新增，**Then** 系統不新增任務，並以明顯方式提示使用者輸入標題。

---

### User Story 2 - 刪除任務 (Priority: P2)

使用者可從清單中移除不再需要的任務，包含待辦與已完成的任務，使清單保持整潔。

**Why this priority**: 刪除功能讓使用者維持清單整潔，是基礎任務管理流程的必要組成，但需在新增功能可用後才有意義。

**Independent Test**: 可在清單中新增任務後，按下刪除按鈕，驗證任務是否從清單中消失。

**Acceptance Scenarios**:

1. **Given** 清單中存在一筆任務，**When** 使用者按下該任務旁的刪除按鈕，**Then** 任務立即從清單中移除，清單其餘項目不受影響。
2. **Given** 清單中同時存在待辦與已完成的任務，**When** 使用者刪除已完成的任務，**Then** 僅該已完成任務被移除，待辦任務保持不變。

---

### User Story 3 - 篩選任務清單 (Priority: P3)

使用者可依狀態（全部、待辦、已完成）篩選顯示任務，以便專注於特定類別的任務。

**Why this priority**: 篩選功能在清單任務數量增加後顯著提升可用性，但不影響核心新增與完成流程。

**Independent Test**: 可在清單中加入待辦與已完成任務後，分別切換「待辦」與「已完成」篩選，驗證是否僅顯示對應狀態的任務。

**Acceptance Scenarios**:

1. **Given** 清單中同時有待辦與已完成任務，**When** 使用者選擇「待辦」篩選，**Then** 清單僅顯示尚未完成的任務。
2. **Given** 清單中同時有待辦與已完成任務，**When** 使用者選擇「已完成」篩選，**Then** 清單僅顯示已勾選完成的任務。
3. **Given** 使用者正在「待辦」篩選模式，**When** 使用者切換至「全部」篩選，**Then** 清單顯示所有任務（待辦與已完成皆可見）。

---

### User Story 4 - 任務資料跨次保留 (Priority: P4)

使用者關閉或重新整理頁面後再次開啟，原本建立的任務清單（包含狀態）依然存在，不需重新輸入。

**Why this priority**: 資料持久化確保任務管理工具具備實用價值；若每次重新整理都清空，使用者將無法信任此工具。

**Independent Test**: 新增數筆任務並勾選部分為已完成後，重新整理頁面，驗證任務清單與各任務狀態是否完整恢復。

**Acceptance Scenarios**:

1. **Given** 使用者已建立多筆任務並有部分標記為已完成，**When** 使用者重新整理瀏覽器頁面，**Then** 所有任務及其完成狀態完整恢復，與重新整理前一致。
2. **Given** 使用者已建立任務，**When** 使用者關閉分頁後重新開啟，**Then** 先前的任務清單仍正確顯示。

---

### Edge Cases

- 使用者輸入超長標題（例如超過 200 字元）時，系統應截斷或限制輸入長度，並提示使用者。
- 清單中無任何任務時，應顯示引導文字提示使用者新增第一筆任務。
- 本地儲存空間不足或被封鎖時，任務功能仍可正常使用（當次操作有效），但儲存失敗時應以靜默方式處理，不影響使用者操作。
- 使用者快速連續按下新增按鈕時，系統應防止重複新增相同任務或空任務。
- 全部任務刪除後，清單應顯示空白引導狀態，而非顯示錯誤或異常畫面。

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: 系統必須允許使用者輸入任務標題並新增至清單。
- **FR-002**: 系統必須拒絕新增空白標題的任務，並提示使用者輸入標題。
- **FR-003**: 系統必須限制任務標題最多 200 字元。
- **FR-004**: 使用者必須能勾選任務，將其狀態切換為「已完成」，並可再次取消勾選恢復為「待辦」。
- **FR-005**: 使用者必須能從清單中刪除任何任務（不論狀態）。
- **FR-006**: 系統必須提供「全部」、「待辦」、「已完成」三種篩選模式。
- **FR-007**: 系統必須將任務清單（含各任務狀態）持久保存於本地，頁面重新載入後資料不流失。
- **FR-008**: 系統必須在清單為空時顯示引導提示，鼓勵使用者新增第一筆任務。
- **FR-009**: 系統必須可部署為靜態網頁，無需後端伺服器。

### Constitution Alignment *(mandatory)*

- **CA-001**: Specification narrative MUST be written in Traditional Chinese.
- **CA-002**: Scope MUST remain pragmatic and minimal; no speculative overengineering.
- **CA-003**: Implementation approach MUST preserve TDD (tests first, fail first).
- **CA-004**: Workflow MUST define phase-based git checkpoints and traceability.
- **CA-005**: Implement phase MUST protect existing spec artifacts (`spec.md`, `plan.md`, `tasks.md`).
- **CA-006**: For website features, default delivery target MUST be a GitHub Pages deployable static frontend unless explicitly stated otherwise.

### Key Entities

- **任務（Task）**: 代表使用者建立的一項待辦事項，具有標題、完成狀態（待辦／已完成）、建立順序等屬性。
- **任務清單（Task List）**: 代表所有任務的集合，支援依狀態篩選顯示，與本地持久儲存關聯。

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 使用者從開啟頁面到成功新增第一筆任務，全流程可在 30 秒內完成。
- **SC-002**: 任務新增、完成、刪除等操作的畫面回應時間在使用者可感知的範圍內（操作感覺即時，無明顯延遲）。
- **SC-003**: 90% 的使用者在首次接觸頁面時，無需說明即可在 1 分鐘內完成新增與勾選任務的操作。
- **SC-004**: 頁面重新整理後，任務資料完整恢復率達 100%（在未手動清除瀏覽器儲存的一般使用情境下）。
- **SC-005**: 清單支援至少 100 筆任務，顯示與操作仍維持流暢，無效能明顯下降。

## Assumptions

- 此功能為單使用者個人任務管理工具，不需使用者帳號或多裝置同步。
- 任務清單以本地儲存機制保存，不需後端服務或雲端資料庫。
- 目標平台為現代桌面與行動瀏覽器，以觸控與滑鼠點擊為主要互動方式。
- 不包含任務截止日期、優先級、分類標籤、子任務等進階功能，以維持範圍精簡。
- 任務不支援排序拖曳或編輯標題（建立後標題固定），這些功能留待後續迭代。
- 清單不需分頁，一次顯示所有符合篩選條件的任務（100 筆以內為合理範圍）。
