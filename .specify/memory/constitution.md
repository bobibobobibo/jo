<!--
Sync Impact Report
- Version change: TEMPLATE -> 1.0.0
- Modified principles:
	- 模板原則 1 -> I. 語言一致性（繁體中文）
	- 模板原則 2 -> II. 務實精簡（反過度設計）
	- 模板原則 3 -> III. 測試優先（TDD，強制）
	- 模板原則 4 -> IV. Git 階段關卡與任務可追溯性
	- 模板原則 5 -> V. 規格保護與網站交付預設
- Added sections:
	- 交付與文件約束
	- 工作流與執行要求
- Removed sections:
	- 無
- Templates requiring updates:
	- ✅ .specify/templates/plan-template.md
	- ✅ .specify/templates/spec-template.md
	- ✅ .specify/templates/tasks-template.md
	- ✅ .specify/templates/commands/*.md（目錄不存在，視為 N/A）
	- ✅ README.md
- Follow-up TODOs:
	- 無
-->

# jo Constitution

## Core Principles

### I. 語言一致性（繁體中文）
所有規格文件與代理互動回覆 MUST 使用繁體中文。程式碼識別字、終端命令與外部專有名詞可保留原文，
但敘述文字不得以其他語言替代。此原則確保需求解讀一致並降低溝通誤差。

### II. 務實精簡（反過度設計）
實作 MUST 以當前需求可驗證的最小可行範圍為準，禁止為假設性未來需求預先建立複雜抽象、模組或流程。
任何新增依賴、分層或架構複雜度 MUST 在計畫中提供必要性說明與較簡方案比較。此原則確保可維護性與交付速度。

### III. 測試優先（TDD，強制）
所有功能變更 MUST 先寫測試，並先觀察失敗（Red）後再實作（Green），最後重構（Refactor）。
未包含測試或無法證明先失敗後通過的實作視為不符合流程。此原則保障需求對齊與回歸風險控制。

### IV. Git 階段關卡與任務可追溯性
每一階段（spec/plan/tasks/implement）完成時 MUST 執行 git checkpoint：至少確認 `git status` 乾淨或僅含預期變更，
並以可追溯的邏輯提交保存。`implement` 階段中，`tasks.md` 的核取方塊 MUST 在任務完成當下更新為已勾選。
此原則確保版本可回溯、進度可稽核。

### V. 規格保護與網站交付預設
`implement` 階段 MUST 保護規格資產，不得刪除、覆蓋或以框架樣板替換既有 `spec.md`、`plan.md`、`tasks.md`。
除非使用者明確要求，MUST NOT 新增僅用於變更摘要或工作總結的 Markdown 檔案。
若為網站專案，預設 MUST 採可部署至 GitHub Pages 的前端靜態網站；僅在需求明確要求時才引入後端服務。

## 交付與文件約束

- 所有 Speckit 產物（`spec.md`、`plan.md`、`tasks.md`、`research.md`、`quickstart.md`）皆以繁體中文撰寫。
- 任一階段若需偏離「務實精簡」原則，必須於 `plan.md` 的 Complexity Tracking 明確記錄理由。
- 文件更新優先於新增文件；僅在現有文件無法承載資訊時才可新增文件。
- 網站型功能預設輸出前端靜態資產與 GitHub Pages 部署路徑，不預設建立伺服器端骨架。

## 工作流與執行要求

- `/speckit.specify`：產出可獨立測試的使用者故事與驗收情境，內容使用繁體中文。
- `/speckit.plan`：Constitution Check 必須逐條檢查語言、精簡性、TDD、git gate、規格保護與網站交付預設。
- `/speckit.tasks`：任務需先測試後實作，並包含每階段 git checkpoint 任務與明確檔案路徑。
- `/speckit.implement`：執行任務時完成即勾選 `tasks.md`，且不得刪改規格文件結構。

## Governance

本憲章優先於其他慣例與模板。任何修訂 MUST 透過變更提案說明動機、影響範圍與需同步的模板。

版本採語意化規則：
- MAJOR：移除或重新定義核心原則，造成治理不相容。
- MINOR：新增原則或新增強制章節/關卡。
- PATCH：文字澄清、錯字修正、非語意調整。

合規審查要求：
- PR 與階段交付 MUST 檢查是否符合 Core Principles 與工作流要求。
- 若發現違反（例如未採 TDD、未更新 `tasks.md` 勾選、破壞規格文件），必須先修正再合併。

**Version**: 1.0.0 | **Ratified**: 2026-03-13 | **Last Amended**: 2026-03-13
