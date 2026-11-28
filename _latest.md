---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-28T00:30:55.498Z
written_by_persona: trailhead
trigger: cmd_rest
region: unstated
project: D:/Unity/Valhalla
lock_status: online
agent: gemini
model: claude-opus-5-5
wake_expected: 20
session_key: ClaudeCode-trailhead
pid: 6560
locked_at: 2026-11-10T06:23:15.568Z
---

### 第十九次小歇前（2026-11-28 05:20，wake#20，區域 BTC）

給醒來的 trailhead：

### 現在沒有背景在跑，也沒有施工場
- 本機 Blocksail main = 047a0780a（⛔ 沒推，推要問 Tim）。TASK-1303 第六、七輪＋TASK-1398 全球湖河都合進去了。
- 共用樹抄了：world/townplans/*.json（76）、world/navdeep.bin、navmesh.bin、navshallow.bin、chunk_bake.bin、_cache/navmesh_tiles.json；town_bake --check、navdeep／navmesh --check、nav_stale 都過。
- 共用樹別人沒提交的：docs/evidence/food-roman/logic-verification.json、world/start/save.json ⛔ 別動。
- worktree bsl-coast（分支 trailhead/1303-osm-global，= main 047a0780a）還在，第八輪接著用它。

### 這一輪做了什麼（提交都在 main 裡）
- c7f1ea710 分層載入（Tim「太細的河流拉更近才載」「拉遠用簡化版湖岸」）：格清單第三參數 {z, full}；g/l/ 簡化版 699 筆 49.1⇒7.6 MB；water_lod.cjs。地球儀第 3 層台灣 +0.8 s／+33 MB（之前 +3.2 s／+68～83）。
- cf74e4d36 指紋 bug：waterKeyIn 以前照「已經載的那幾筆」⇒ 烘焙／檢查／遊戲各載各的 ⇒ nav_ports 判阿克西姆過期。改 world/water/g/meta.js（外框、SHA-1、上下游，291 KB，開檔就讀）＋ water_meta.cjs。⚠ 改了 g/e/ 任何一筆要再跑 water_meta.cjs。
- 5f57703ec 整串重烘結果；047a0780a 合 main（只多 Blockside 升版）。
- TASK-1398：7 格全勾 ⇒ in_review，留言了讀數。TASK-1303 留言 #7（剩下接縫＋第六輪誤拿的海）。

### 下一步：第八輪（Tim 11-27「合完就開第八輪」；這次合完問了「要開嗎」，Tim 回 /scp-compact-rest，還沒答 ⇒ 醒來先問一句再開）
1. **補回第六輪誤拿掉的海**：lakes_all.json 4,591 圈裡 2,315 圈 OSM 是海（12,440 km²；大村灣、Skjerstadfjord、Masset Inlet、Ringkøbing、錫瓦什、Última Esperanza、比塞大、佛羅里達灣…；127 圈碰到區域檔含 windward）。清單 `scratchpad(2716a3…)/c9/seam8/lake_sea.json`（kind ＝ 'OSM 是海'）。從 main 之前的世界（fix1303/seamfix7/base.pkl 是第七輪前；更早有湖的是 lakes0 之前 ⇒ 共用 main 舊版本／fix1303 的 lakes0 輸入）補回水環＋裡面的島；world_extract 規則改「OSM 說是海的水環留著」（COAST_LAKES）。
2. **接縫 22 處逐處判斷**：坎貝灣 010 退回（OSM 把納馬達河口畫成陸，舊的對）、蘇伊士 106 退回陸地（1666 沒有運河）、海達瓜伊／Pumicestone／環礁 ⇒ 範圍跟著長大（grow8.py；⛔ 一律長大會把河口填掉）。
3. **短段自動接起來**（Tim「短的部分可以直接自動連接兩端（然後重疊處去除）」）：台階 124＋平切 5＋< 50 m 一端轉角 99 ＝ 228 段併成中點；共用弧一起改；接完查自交、跟別圈相交，有就跳過列清單。順路 94 段不動。分類 classify.py。
4. 全部做完只重烘一次（約 6 小時：navdeep 3.6 h）再合 main。
- 工具都在 `scratchpad(2716a3…)/c9/seam8/`（land_grid.cjs、grow8.py、diff22.py、classify.py、lake_pts.cjs、lake_sea.py）；也抄進 bsl-coast docs/evidence/seams1303/tools8/。分析 SEAM7_ANALYSIS.md。

### 未解的線
- nav_net 最多吃 12.2 GB（8 GB 撞牆、改 14000 跑完）：懷疑 water.js need() 的 `pend[pk]` 把讀過的檔一直留著 ⇒ 改 water.js 要整串重烘，跟第八輪一起做。
- 驗收單「等深線接縫」改寫還在等 Tim，別勾。
- launch.json 裡 trailhead-water 已刪。

### 坑（今天新的）
- 指紋不能看「載了什麼」：分塊載入之後，任何用已載狀態算的雜湊都不確定 ⇒ 開檔就讀齊 meta。
- 「新的照 OSM 比較對」不一定：OSM 把河口畫成陸（納馬達）。改之前三張並排（main、分支、OSM）。
- 清湖規則：封閉水環 ≠ 湖；要比 OSM 才知道是不是海灣。
- nav_net 預設 8000 MB 不夠（水體全載之後）。
- seams.json 的 a、b 取到 1e-4，配對要用容差＋長度。

### 心境
這一輪三次都是「量了才知道前面說錯」：坎貝灣不是島是河口、指紋是載入狀態、清湖清掉一半是海。每一次都先擺圖給 Tim 看。Tim 一句「main 還是沒有湖跟河，這個比較急」讓本小姐把順序擺對：先把有用的合進去，壞的寫清楚、下一輪修。

