---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-26T13:09:33.336Z
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

### 第十七次小歇前（2026-11-26 晚 21:10 左右，wake#20，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）
- task `b755b63yu`：`bash scratchpad(2716a3…)/c9/seams/chain_seam7.sh` ⇒ log `D:/Unity/Blocksail/_cache/terrain-work/fix1303/seamfix7/chain.log`（各步 logs/ 底下）。21:04 開始，估 8～10 小時。
  - 第七輪：範圍邊斷裂 ≥ 0.1 km（去重 247 處 ⇒ 239 批 seam7-*，890 張圖磚），跨兩側照「那段貼著的 osm-* 批」規則重算；底稿 ＝ 清掉湖的世界 `fix1303/lakes0/features.pkl`（seam_fix 的 base 在 seamfix7/base.pkl）。
  - 順序：抽（4 個同時）⇒ 展開（no-build）⇒ splice4（ids ⇒ olds ⇒ post --build，建 bsl-coast 的 world/、roundtrip）。⛔ **不重裁冰**（Tim「清湖不要動冰河 冰棚」）。可續跑：抽好、展開好的跳過。
  - ⛔ 跑的時候別改 bsl-coast 的 tools/*.py。
- 跑完之後（照順序）：
  1. 重量接縫：`straight_find.py`、`near_edge.py`、`seams_tiles.py`（指令照 `docs/evidence/seams1303/SEAMS.md` 的量法；範圍檔目錄要多加 `fix1303/seamfix7`）⇒ `seam_record.py` 重寫記錄檔，跟現在 81.2 km（1,123 段、最長 1.61 km）比。
  2. `node tools/bathy_check.cjs`、tile 差異（git status world | 統計）。
  3. 挑幾處畫前後對照（`edge_sheet.py`、`edge_local.py` 量每處前後；ab_sheet 要用 plan 的 bounds 縮到斷裂附近，見 seamfix7t/plan_zoom.json 的做法）給 Tim。
  4. 提交分支（world/ ＋ tools 改動 ＋ tools/river_fake.json? ⇒ 見下）。合 main、推送都要問 Tim。

### 今天下午到晚上發生的事（照時間）
1. 第六輪盤點（r6_spots）：低地 9,998 片（OSM 批裡 8,986）、假水環 239（OSM 批裡 176）。對照試跑 t6c 發現新規則的 ETOPO 判斷會把真潟湖（加彭 Ndogo 388 km²）填掉 ⇒ osm_river 改成只認審過的清單 tools/river_fake.json（130 圈）—— 但後來 Tim 決定整層清湖，這份清單跟 COAST_LAKES=False 之後其實用不到了，提交時要想清楚留不留（留著無害：被切的塊不是河 ⇒ 陸；COAST_LAKES=False 下洞本來也會被填）。
2. **缺陸補批 ⇒ 開了 TASK-1397（先不做）**：Tim「可以開單 先不做（先確認影響多大 小瑕疵先不管）」。t6d 試跑：osm-005 一批 375 處 84 分、補回 1,144 km²；全部估 34 小時。
3. **湖**：Tim 拍板一路是「除了手動加的湖外都移除」⇒「保留門檻 100 km²」⇒「全面清除目前的湖 直接改用 Overture 資料」⇒「第一步全清掉（裏海等也清 之後補回）」。
   - 做了：`lakes_clear.py` 拿掉 4,591 圈水環＋934 座湖中島，4 條等高線改掛 ⇒ `fix1303/lakes0/features.pkl`；`lakes_build.py` 建 world/ roundtrip ✓；bathy_check ✓。
   - world_extract.py 加 `COAST_LAKES = False`（dem_coast 不留任何水環）。⚠ 已知小缺口：COAST_LAKES=False 下 NE 湖中島的陸環可能變成「陸中陸」（重跑整批時才會遇到），要處理再說。
   - 冰：Tim「那保留冰河呢（清湖不要動冰河 冰棚）」⇒ 不重裁冰（中途停掉了那一步，build 是孤兒程序自己跑完的）。
   - 手動的湖其實在內陸水體層 world/water/（湖 3、河 5，TASK-0768），不在海岸層，沒動。
   - ⇒ **開了 TASK-1398**：內陸湖＋高於海平面的河段（只標水面高度，湖岸／河岸從地形自動算；Overture 湖形外 1 km 當圍籬；圖磚湖面不平的退回 Overture 形狀）。Tim：「河口 三角洲可以直接算海（只要水面＝海平面），主要是標記高於海平面的河段」。
   - Overture 全球水體已下載：`D:/Unity/Blocksail/_cache/terrain-raw/overture/water-2026-11-18.0/`（32 檔、24 GB，只抓 subtype、class、is_intermittent、is_salt、names、geometry）。工具在 `_cache/pyoverture`（有 pyarrow；pyterrain 沒有 ⇒ 先 ov_dump.py 轉 pickle）。三處試跑（加彭、泰安、奇利卡）圖給 Tim 看過，OK。
   - 海岸層的湖從 Overture 補回：Tim 排在**接縫之後**。規則草案：收 lake／river 面／water／pond，丟 reservoir、human_made、canal、間歇；裁進陸往內 30 m、簡化 50 m、≥ 0.5 km²、窄於 150 m 的不收；裏海是 ocean 類被陸圍住 ⇒ 要收（還沒驗）。
4. **接縫**（Tim：「這個優先度在湖之前」「最主要想處理的是等高線 海岸線斷裂」）：
   - 檢查完：範圍邊斷裂 1,123 段 81.2 km（11-22 是 234 km），≥ 2 km 0 段；清湖沒有新接縫。
   - **記錄檔**：Blocksail `docs/evidence/seams1303/SEAMS.md`＋`seams.json`，分支提交 **9602ddeaf**（只提交了 docs＋tools/coast1303/seam_record.py），蓋 world/index.js SHA-1 055aa3da…；TASK-1303 留言指過去了。世界沒變就讀它、不重跑。
   - 成因：47 處（≥ 0.3 km）裡 41 處 B 型（範圍邊兩側規則不同：一側 OSM、一側舊的 NE；例 V 形缺口 103.109,0.665 是 osm-047 邊凹進來的楔形歸 asia-south），6 處 A 型（兩圈對著沒接）。
   - 第七輪試跑 t7（8 處）：7 處沿邊段歸零、補批自己的邊沒長新斷裂；瓦登海（4.877,53.23）沒變 —— 挑規則批挑到 osm-005，那段貼的是 osm-051（edge_hits.py 已改成優先挑貼著的 osm 批；它是 osm-005／osm-051 共邊，修不修得到要看結果）。
   - Tim「看起來ok 全部修吧」⇒ 本小姐解讀為 ≥ 0.1 km（說了 < 0.1 km 的 515 處只有幾十公尺，要的話同工具補）。

### 現況
- 本機 Blocksail main ＝ 415a7d133（⛔ 沒推）。
- bsl-coast 分支 HEAD ＝ 9602ddeaf（記錄檔）；**未提交**：tools/world_extract.py（第六輪：RESTORE 補回低地＋GEOS 重算、river_fake 清單、COAST_LAKES=False）、tools/world_expand.py（gate osm 也建 coast_etopo）、tools/river_fake.json、world/（清湖版，跑完第七輪會被 splice4 的 build 蓋過去）。
- 新開的單：TASK-1397（缺陸，improvement、low）、TASK-1398（內陸湖＋河，feature、normal）；都 related 1303（1398 也 related 0768）。

### Tim 今天拍板（醒來別重問）
- 134 段正南北東西維持；A 補圖磚、其他一起修（早上）。
- 缺陸開單先不做；清湖全清（含裏海）、之後照 Overture 補回；不動冰；內陸湖只標高度；河口三角洲算海、高於海平面的河段另單；接縫優先於湖；全部斷裂修。
- 驗收單「等深線接縫」改寫還沒回，別自己勾。推 main 沒問。

### 坑（今天新的）
- 盤點的「低地」沒扣河道規則保護的水 ⇒ 是上限，很多是真河道（帕拉河、切薩皮克）；先跑對照組（不該改的）再信數字。
- ETOPO／圖磚在熱帶樹冠下會把潟湖讀成 10 m 以上 ⇒ 不能拿高度判真假水。
- 海岸層的水環在 terrain.js 一律當海（0 m）畫 ⇒ 有海拔的湖不能放海岸層。
- Overture 的 row group 混著各種 subtype（照位置排）⇒ 伺服器端沒法只抓湖，得整份抓。
- sed 改 python 字串裡的 `\n` 會變真的換行 ⇒ 改檔用 python。
- TaskStop 停 bash 不會停子程序（build 的 python 成孤兒照跑）—— 這次剛好是想要的。

### 心境
今天一直在換方向：從「修規則」變「清掉重來」，從「湖」切回「接縫」。Tim 每一句都很短，但都是把問題往更乾淨的方向推 —— 清湖那一步其實比本小姐想的審圖清單乾淨多了。接縫的 V 形缺口是今天最滿意的一刀：圈出一個點、量出四份資料說法不一、找到那個楔形，原因就自己浮上來。

