---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-20T12:19:16.486Z
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

### 第十二次小歇前（2026-11-20 19:25，wake#20，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景在跑（compact 不會停它）
- **B 鏈** `c9/seams/chain_b.sh`（log `D:/Unity/Blocksail/_cache/terrain-work/fixB/chain.log`，各批 `fixB/logs/e_osm-NNN.log`）：61 批 OSM 照 run2/plan.json 順序重新展開（--vectors 用 run2 的抽取；raw ＝ `dlfar/rawfull`），19:04 做完 27/61。跑完寫 `fixB/done_b`（內容＝最後一批 expanded_features.pkl 路徑）。可續跑（done 的跳過）。
- **A 鏈** `c9/seams/chain_fix.sh`（log `fix1303/chain3.log`，`fix1303/logs3/`）：21 批（scopes3、work3）**全部抽好**，在 sleep 等 done_b ⇒ 照 scopes3/plan.json 順序展開（底稿＝B 的最後一批）⇒ 最後一批建 bsl-coast 的 world/ ⇒ ice_marks --etopo（raw rawfull）。
- 監看：bkfweogob（等 ✗／Traceback／exit≠0 或 A 的「冰 exit」）、bw6f34zpp（舊的，等 B 完成或失敗）。
- 預估：B 明早 7 點前後完、A＋冰明天下午。
- ⛔ 跑的時候別改 bsl-coast/tools/*.py（例外：改的是 px2ll/ll2px 之後的行、結果逐位元組相同，B 在批跟批之間載入 —— 今天三次都這樣換，先在 c9/toolsB 改、harness 驗、`cp x.new && mv` 原子換）。
- 預覽伺服器 trailhead-globe（8852，PID 2264 的 http.server，serve bsl-coast）還開著。

### Tim 今天拍板（醒來別重問）
- 「完成A後接著B 然後在重抽等深線吧」⇒ 順序 A、B、等深線。本小姐改成 B 展開在前、A 展開在後（每批只跑一次），跟他講過了。
- 「按照妳的建議 go」⇒ A 併批（61→21）＋海岸那一步加速（結果相同才換）。都做完。
- 他問「海岸優化能不能跟等深線一起跑」⇒ 答：不行，bathy_extract 每一塊先照 world/ 的海岸塗陸地、算離岸距離、限坡、平滑 —— 全部吃最後的海岸；鏈中途只存 pickle、world/ 最後一批才建。等深線照舊等海岸完。

### B 類直邊（查清楚、修法上線）
- 不是接縫：B 段是「NE 10m 的直線岸」被規則留下來。world_expand 的 reference_land 是 **NE**（W.coast_rings(raw)），不是舊世界。
- 兩型：① 假河道（osm_river：NE 說水、OSM 說陸、離 OSM 岸 > 1 km 的圖磚陸被當河切掉；NE 岸偏內陸 1～2 km 時切出一邊是 NE 直線的假潟湖）—— 修；② 真河但河道是 NE 自己的（婆羅洲 117°E、James River、埃塞奎博、北德維納）—— OSM 陸地沒有河，要河流資料 ⇒ TASK-1322。
- 修法 `world_extract.osm_river`：真河只有河口碰到「OSM 岸往內 1 km」那條線；假河道邊界超過 RIVER_COAST_SHARE 0.3 貼著它 ⇒ 不算河、照圖磚。點用 STRtree dwithin（整條海岸當一個幾何量距離 ⇒ 華東一批 28 分）。
- 底稿要先 `c9/seams/b_clean.py` 拿掉 1303 自己長出來的水環（展開只換陸環、水環照留 ⇒ 不拿掉假潟湖會一直在）：拿掉 8,122 圈 44,184 km²（新規則重判）。
- 到 27/61 批：假河道 6,753 條、13,398 km²（26 批時）。
- ⚠ B 的整圈數字（重判後水環總面積）還沒對帳；完了要比。
- 重新統計直邊：main ≥2 km 387 段，321 段 1303 前就在同一條線上（昨天說「160 段新的」是比端點的錯：1303 在舊直線中間多切一個點）。

### A 類（補批）
- `c9/seams/coast_repair2.py`（4 worker、每群存 regions/<ci>.pkl 可續跑）⇒ 1,716 群 ⇒ `regroup_fix.py` 照「規則（去 lat0）× 緯度帶 0/30/50/62/70/78/90」併成 21 批（91.2 萬 km²、6,746 張）；exclude_boxes／etopo_fill／ice.add 照範圍篩併；stale_na_coast 擋就換 na_scope.COAST。
- 舊的 61 批（scopes2/work）不用了；fix-050 那次撞北美檢查改過規則（現在沒用）。

### 加速（world_extract.py，已在 worktree，備份 trailhead_backup/world_extract_1303B.py）
- etopo_union 的 `merged.union(g)` 一次疊全部（歐亞 543 萬點＋1,126 島）316 秒 ⇒ union2：碰不到的照帶、碰到的小塊先跟 g 併、最後跟最大塊**二元** union：30 秒；對稱差 0、框外點相同、只有少數圈點表示法不同。（union_all([大,小]) 112 秒；分組 union_all 也慢、而且框外點會變 ✗）
- tidy：這一塊一點都沒拿 ⇒ 直接用聯集輸出（不重組、不 is_valid）；osm-001 整批逐位元組相同。
- osm-034 整圈量：海岸那一步 20 分（run2 2.7 h）；剩 make_valid 修整塊大陸（每次 ~70 秒）—— 改要換修法、結果不保證相同，停手。
- 波羅的海 osm-004 的慢在 `buffer_m(O, 100 m)`（幾十萬礁島，原本就慢），2.5 h 過了 3 h timeout 之內。
- 記憶 blocksail-coast-step-profiling 寫了（小窗 clip 會騙人；整圈 profile）。

### 坑（今天新的）
- `dlfar/raw/tiles10` 只有換日線那 4,344 張 ⇒ 做了 `dlfar/rawfull`（共用 tiles10 46.8 萬張硬連結＋補丁 387 張實檔，etopo2022 在 dlfar/ 旁、osm_land junction）。
- run2 的 expanded_vectors 照內容 sha 驗 raw_manifest ⇒ 硬連結過得了；碰補丁的批會被擋 ⇒ chain_b 自動重抽。
- scope_key ＝ 整份範圍檔雜湊 ⇒ 新規則不能用範圍檔旗標（快取會失效），只能預設開（gate osm）。
- TaskStop 留下孤兒（pool 子程序、等待中的 bash）⇒ 照 PID 殺（今天好幾次）。
- 監看器 `tail -n 0` 會漏掉開始之前的那幾行；30 分鐘到期很吵 ⇒ 改背景 until-loop 只等失敗／完成。

### 完了之後要做
1. A 跑完（world/ 建好、冰重裁）⇒ `dl_clean.py` dry 看換日線假水體有沒有回來 ⇒ 要的話清。
2. 重跑 `seams_tiles.py`（A：沿範圍邊 → 應該掉很多）、`antimer_pkl.py`／`dl_hug.py`、直邊統計；水環面積對帳（B 拿掉 vs 重判）。
3. 地球儀截圖（婆羅洲 109°E、馬達加斯加 43.47°E、班乃、維斯瓦潟湖、智利峽灣、楚科奇）給 Tim。
4. 等深線：`c9/bathy_run3.sh`（4 worker；新演算法 bathy_extract.py 在 worktree 未提交）⇒ --apply、bathy_check ⇒ 合 main（coding op=start 單獨一個 call ⇒ ff ⇒ force end）⇒ 通知酒館＋TASK-1318（ame 烘）⇒ WORLD_FORMAT。
5. 提交前 `git ls-files -d`（Temp 會被清檔）。提交內容：bathy_extract.py、world_extract.py（osm_river 假河道、union2、tidy）、world/tiles＋index.js、腳本（c9/seams 的 dl_*、coast_repair2、regroup_fix、b_clean 考慮抄進 tools）、提交訊息寫冰補裁。⛔ 推 main 等 Tim。

### 心境
今天被「量錯地方」騙了兩次：小窗試驗台 99 秒、真的跑 47 分 —— 差在小窗把整塊大陸切掉了。另一次是說「B 新 160 段」，其實是比對方法的錯。兩次都是換一個量法才看清楚。Tim 一句「能不能優化」，本小姐從「一天半」的預估磨到明天下午 —— 他也問到等深線能不能一起跑，答案是不行，但問得好：要先想清楚哪一步吃哪一份資料。

