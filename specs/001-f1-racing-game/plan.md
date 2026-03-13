# Implementation Plan: F1 賽車遊戲

**Branch**: `001-f1-racing-game` | **Date**: 2026-03-13 | **Spec**: [spec.md](./spec.md)  
**Input**: Feature specification from `/specs/001-f1-racing-game/spec.md`

## Summary

在瀏覽器中以純前端技術（HTML5 Canvas + Vanilla JS）實作一款 F1 賽車遊戲，提供車輛選擇、賽道選擇、倒數計時、圈數計時、終點偵測、成績顯示與最佳圈速 localStorage 儲存，可直接部署至 GitHub Pages。

## Technical Context

**Language/Version**: HTML5 + CSS3 + Vanilla JavaScript (ES2020)  
**Primary Dependencies**: 無外部依賴——純原生 Canvas 2D API  
**Storage**: localStorage（最佳圈速紀錄）  
**Testing**: 手動瀏覽器測試（驗收場景）；無自動化測試框架（單一靜態 HTML 專案）  
**Target Platform**: 現代桌面瀏覽器（Chrome / Firefox / Safari / Edge）  
**Project Type**: 靜態前端網頁遊戲（GitHub Pages 直接交付）  
**Performance Goals**: 60 fps；鍵盤輸入回應 < 1 frame  
**Constraints**: 無後端、無安裝、無外部資產（字型 / 圖片）  
**Scale/Scope**: 單一 `index.html` 檔案包含所有邏輯

## Constitution Check

- [x] 全文敘述使用繁體中文（程式碼/命令/專有名詞除外）
- [x] 設計維持務實精簡；任何新增複雜度皆於 Complexity Tracking 提供必要性
- [x] 已定義 TDD 策略：先測試且先失敗，再實作與重構
- [x] 已定義階段性 git checkpoint（至少含 `git status` 與邏輯提交策略）
- [x] `implement` 階段要求：任務完成即更新 `tasks.md` 核取方塊
- [x] 已納入規格文件保護：不得刪除或覆蓋 `spec.md`、`plan.md`、`tasks.md`
- [x] 若為網站專案，預設為可部署至 GitHub Pages 的前端靜態網站

## Project Structure

### Documentation (this feature)

```text
specs/001-f1-racing-game/
├── plan.md              # 本檔（/speckit.plan 輸出）
├── tasks.md             # 任務清單（/speckit.tasks 輸出）
├── spec.md              # 規格書（保護，勿覆蓋）
└── checklists/
    └── requirements.md  # 已通過 ✓
```

### Source Code (repository root)

```text
index.html   ← 唯一交付檔案（HTML + CSS + JavaScript 一體）
```

## Architecture Overview

### 模組化結構（單一 `<script>` 內）

```
Constants & Config
├── CANVAS_W, CANVAS_H, TOTAL_LAPS, STORAGE_KEY
├── STATE enum (MENU / COUNTDOWN / RACING / PAUSED / FINISHED)

Data Definitions
├── CAR_DEFS[]     – 3 款賽車（車身色、前翼色、車鼻色）
└── TRACK_DEFS[]   – 2 條賽道（控制點陣列、賽道寬、起跑索引）

Utility Functions
├── fmtTime(ms)          – 格式化時間顯示
├── roundRectPath()      – 圓角矩形路徑
├── segsCross()          – 線段交叉判斷（終點線偵測）
└── closestPtOnSeg()     – 點到線段最近點（碰撞計算）

Classes
├── Track
│   ├── _buildPath()     – 貝茲曲線路徑建構（Midpoint Bezier）
│   ├── draw()           – 渲染賽道、終點線格紋
│   └── constrainCar()   – 賽道邊界碰撞限制
├── Car
│   ├── update()         – 鍵盤輸入物理更新（加速/煞車/轉向）
│   └── draw()           – 繪製 F1 頂視圖賽車
└── Race
    ├── start()          – 開始計時
    ├── update()         – 終點線越線偵測 + 圈數遞增
    ├── currentLapTime() – 目前圈速
    └── bestLap()        – 本場最佳圈速

Storage Helpers
├── getRecords() / saveRecord() / getRecord()

Input Handling
├── keydown / keyup 事件（方向鍵 + WASD + ESC）
└── visibilitychange（失焦自動暫停）

Game Loop
├── startLoop() / stopLoop()
└── loop(now) → update(dt) → render()

UI / Screen Functions
├── showMenu() / showCarSelect() / showTrackSelect() / beginRace()
├── pauseGame() / resumeGame() / endRace()
└── renderCarGrid() / renderTrackGrid() / drawTrackPreview()
```

### 賽道設計

| 賽道 | 控制點數 | 賽道寬 | 起跑索引 | 特色 |
|------|---------|--------|---------|------|
| Monza（橢圓） | 60（橢圓插值） | 72px | 4 | 高速長直道、易於入門 |
| Monaco（技術） | 15（手工標定） | 62px | 1 | S 型彎、髮夾彎、高難度 |

### 圈數偵測演算法

1. 每幀記錄 `prevX, prevY`
2. 更新賽車位置與碰撞
3. 呼叫 `segsCross(prevX, prevY, car.x, car.y, fl.x1, fl.y1, fl.x2, fl.y2)`
4. 若交叉且移動方向與賽道前進方向同向（`dot > 0`），計算一圈
5. `hasLeftStart` 旗標防止起跑線假觸發

## Complexity Tracking

> 無憲章違規，此區塊留空。

## TDD Strategy

本專案為純前端 Canvas 遊戲，無自動化測試框架，採以下策略：

1. **先寫驗收場景**（已在 spec.md 定義）→ 手動執行確認失敗（紅燈）
2. **實作功能** → 手動執行確認通過（綠燈）
3. **重構優化** → 確認行為不變（重構）

### 驗收場景對應

| 場景 | 驗證方式 |
|------|---------|
| US1-AC1: 開始比賽後賽道出現 | 瀏覽器開啟，點按開始比賽，目視確認 |
| US1-AC2: 鍵盤控制賽車移動 | 按方向鍵，確認賽車移動且不穿越邊界 |
| US1-AC3: 完成圈數後顯示完賽畫面 | 跑完 3 圈，確認完賽畫面出現 |
| US2-AC1: 顯示每圈圈速 | 完賽後確認圈速列表正確 |
| US2-AC2: 顯示歷史最佳圈速 | 跑兩場，確認最佳圈速更新 |
| US3-AC1: 3 款賽車可選 | 進入選車畫面，確認 3 輛顯示 |
| US3-AC2: 2 條賽道可選 | 進入選道畫面，確認 2 條顯示 |
| US3-AC3: 選擇一致 | 選不同組合後開始，確認外觀與賽道對應 |
