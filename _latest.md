---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-15T10:30:48.501Z
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

### 第八次小歇前（2026-11-15 18:30，wake#20，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）
- **全球 OSM 串接**（task b0tj8th7n；監看 b0g98ub7x 30 分鐘會過期，要再掛）：`sh D:/Unity/Blocksail/_cache/terrain-work/osm/chain_osm.sh <worktree> <osm>/run1 <osm>/base.pkl`。
  - log：`osm/run1/chain.log`（每批 start／done／✗），每批 `osm/run1/logs/e_osm-NNN.log`。
  - 61 批，照 `run1/plan.json` 的順序，第一批是 osm-016（中國沿海，18:17 開始）。估 12～15 小時。
  - 可續跑：有 `run1/<批>/done` 的跳過；只留最後兩批的 expanded_features.pkl。斷了就同一行再跑。
  - ⛔ 跑的時候別改 worktree 的 tools/*.py。
- worktree `C:/Users/Tim/AppData/Local/Temp/claude/8b51c3eaa66b47c0/568ad2a5-a4f2-42a7-ab88-8f11d10e2bd4/scratchpad/bsl-coast`，分支 **trailhead/1303-osm-global**：
  - dc3e3bee：工具 coast_batches.py＋加速 mv_ring＋scope_of 收 {ring, holes}。
  - cb6a6e93：merge_far 加一條「伸出父環的點都在海上就照收」。
  - 都還沒合 main。
- c9 ＝ `C:/Users/Tim/AppData/Local/Temp/claude/2716a3acd6803927/568ad2a5-a4f2-42a7-ab88-8f11d10e2bd4/scratchpad/c9`（glob/ 底下：plan、除錯 dbg/、size、added_land）。
- 預覽伺服器 trailhead-globe（port 8852）指著 worktree；launch.json 裡那一筆是我加的，收工只刪自己那筆。

### 本機 Blocksail main = 4995096c（⛔ 沒推）
- 台灣第二輪（整島範圍＋OSM 外補回來的陸一律裁掉）。
  - d72c5185 第一輪：框切過恆春、圖磚清單少一列、NE 內縮的假陸地 ⇒ Tim 在地球儀上抓到。
  - 02270ea8 第二輪修好，再合 main 15fa9bf7（gura 的 Blockside 貢多拉）。
- 共用區 townplans 42 份已抄（只換指紋）；town_bake --check 0 ✗、bathy_check ✓；施工場已 force 收。
- ⚠ 共用區的 battle.html、external/Blockside 有改動，那是 gura 的（子模組在 0fcf6e3 1291-seawall），⛔ 不要動。
- TASK-1303 已認領（in_progress）。TASK-1246 照舊 in_progress。

### Tim 今天拍板（別重問）
- 「台灣OK了 接著開始全球重建吧」。
- 「等高線不用全部重抽 應該只要抽沿海地帶(被海岸線裁切掉部分 跟海岸線一起修)」⇒ 本小姐用沿海 ±5 km 的帶，海岸、等高線一起重抽（一塊約 16 s，可行）。
- 「是否有辦法優化算法 去除這種虛空突起」⇒ dem_coast 最後照 OSM＋100 m 裁（已做、已合）。

### 跑完之後要做
1. 最後一批的輸出 ⇒ carry_splice（底稿 fix12 那 18 圈）⇒ build10 建 worktree 的 world/。
2. 量：
   - 全球 bulge（OSM＋150 m 外的陸，c9/tw/bulge.py）。
   - 批的邊有沒有直牆（海岸、等高線貼著範圍邊走）。
   - contour_coast_check。
   - 地球儀抽查：中國沿海、挪威、加勒比、格陵蘭。⚠ 換埠號或 Ctrl+Shift+R，快取騙過我一次。
3. 等深線：海岸變動 ≥ 1 km² 的群重抽；換日線改成同一次抽取跨線再切開。
4. 規劃重烘、文件、提交、合 main。

### 今天的坑
- 範圍檔照抄別區的圖磚清單 ⇒ 少列就變海。一律照「範圍裡有下載的全部」。
- 閘門之後還有三條路補陸（NE 內縮、NE 小島、ETOPO 留回）。
- 巨圈（歐亞 50 m）碰到窗口就整圈重新裁進海岸 ⇒ 遠處的子圈會伸出父環（在海上）。
- 合 main 前只核了「格子＝特徵檔」沒在地球儀上看，把錯的版本合了進去。以後每一批：
  - 開跑前量範圍邊切過的海岸環。
  - 跑完量 OSM 外的陸、畫對照圖再建。

### 心境
今天被 Tim 的截圖抓了兩次：直角缺口、尖端被切。兩次都是本小姐自己的檢查只看「資料一致」，沒看「長得對不對」。修完那張恆春的圖，西岸貼著灰色的 OSM 走，那一刻才算真的修好。快取那一下也很丟臉，好在先換埠號再看，沒有把對的東西又拆掉。

