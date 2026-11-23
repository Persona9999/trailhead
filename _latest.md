---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-23T13:32:00.326Z
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

### 第十三次小歇前（2026-11-23 21:35，wake#20，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景在跑（compact 不會停它）
- **範圍邊接縫補批** `c9/seams/chain_seam.sh`（log `D:/Unity/Blocksail/_cache/terrain-work/fix1303/seamfix/chain.log`，各批 `seamfix/logs/e_seam-NNN.log`）：seams_tiles 量到 ≥ 2 km 的 16 段併成 11 處（`seam_fix.py`：跨兩側外擴 10 km、規則照那一段所在的 run2 批、北美換 na_scope.COAST），底稿 `seamfix/base.pkl` ＝ final_world.pkl 扣 dl_clean 的 54 條、再扣碰到範圍的 11 圈水環（交給規則重判）。21:30 展開到 4/11（一批 11～31 分）；最後一批建 **bsl-coast 的 world/** ⇒ ice_marks 重裁冰（~1 h）。
- **等深線群接縫（帶每段座標）** `c9/seams/bathy_seams2.py` ⇒ `fix1303/bathy_seams_segs.json`（rows 第 8 欄 ＝ 錯開的每一段 ≥ 0.2 km）。
- 預覽伺服器 trailhead-globe（8852，serve bsl-coast）開著。⛔ 跑的時候別改 bsl-coast/tools/*.py（例外規則見舊信）。

### 今天做完（Tim 11-22～23）
- A 21 批、冰、world/ 建好；dl_clean 清換日線 54 條等高線細條（7 格子檔）；全球等深線 97 群重抽（an64a、isspp 本來就沒檔）。
- `--apply` 加速：陸地裁到改線範圍外 0.15°（EXT_CLIP）⇒ 13 h 11 m ⇒ 12.5 m，對照原算法對稱差 9,192 m²；OSM 閘門先裁 0.1° 再擴；「挖回 km²」統計照第一次聯集。
- **合進本機 Blocksail main**：8ff5d5ae4（1303 續）＋2702d227a（合 main）＋6b24e7104（孤立深坑：威尼斯 4 處、錫瓦什東半，16 筆 shoal）＋d7a3be122（合 main）。共用區 `_cache/bathy/base` 換新（舊的在 fix1303/shared_bathy_base_pre），共用區重套 --apply 0 檔變動。施工場兩次都 force 收了。TASK-1303 留言、酒館 seq 35816（請 ame 照 TASK-1318 烘）。
- ⛔ **沒推**：main 現在 36c06e60d（別人又往前），比 origin 多 11 筆（含別人的）。Tim 還沒說推。
- 記憶寫了 feedback-blocksail-terrain-not-bitwise（Tim「不需要逐位元相同 遊戲內玩起來正常就好」）。

### Tim 拍板（醒來別重問）
- 「不需要逐位元相同 遊戲內玩起來正常就好」⇒ 加速差幾公尺直接換、報讀數。
- 「等原版跑完吧」（--apply 對照）、「合進本機 main」、「先修明顯是假的吧」（孤立深坑只修威尼斯 #4～#7、錫瓦什 #13）、「先修範圍邊」（換日線之後再說）。

### 讀數（給 Tim 看過的）
- 範圍邊直段：main 6,884 段 4,918 km（≥2 km 443）⇒ 1,378 段 234 km（≥2 km 16，8 段 main 就有）。審查圖 `c9/pits/seams_01.png`（#1～#16 範圍邊、#17～#28 換日線）。
- 換日線（antimer_pkl）：471 段對不上（上一輪 480）——楚科奇 200～500 m 層 2.5～3.9 km、斐濟塔韋烏尼 0～700 m 層 ~3 km、南極 −84° 0／100 m 層 4～5 km；細條 11 圈。**原因沒查**；Tim 選了先修範圍邊。三條路跟他講過：查原因（先斐濟）／切口後處理拉齊／楚科奇南極接受。
- 等深線群接縫：118 對、82 層、**10.1 km（驗收基準 8.1 ⇒ 沒過）**，最長 ean56｜ean64 10 m 1.23 km；換日線 2 層 0.1 km。座標版在跑，畫成審查圖再跟 Tim 討論。
- 孤立深坑（pit_find／pit_sheet）：水上 6,822 個 ⇒ 3,777 群；城鎮 30 km 內 12、換海岸露出 56 ⇒ 審查圖 `c9/pits/pits_01..04.png`。威尼斯北邊那條 **1303 前就在**（不是換海岸，是 venice-lagoon 範圍沒包到）。錫瓦什西半是封閉湖環 ⇒ TASK-1322。B 組（峽灣、深水道）留、C 組（開闊淺海、三角洲外）等 Tim 判。

### 補批跑完之後
1. `seams_tiles.py` 重量（照 final 的指令；world 換成 bsl-coast/world）、`seam_sheet.py` 畫前後對照給 Tim。
2. 海岸改過的 11 處所在的等深線群重抽（`bathy_extract --only …`）＋ --apply ＋ bathy_check；換日線 `dl_rings_from.py`＋`antimer_pkl` 再量一次確認沒變差。
3. 提交（先 `git ls-files -d`；world/tiles、bathy 被刪的要對新索引）⇒ coding op=start（單獨 call）⇒ 合 main ⇒ ff ⇒ force end。main 又動了。
4. 等深線群接縫審查圖給 Tim（bathy_seams_segs.json 第 8 欄）。
5. TASK-1303 驗收 11 格全沒勾：分批、可續跑、全球讀數、文件可以寫讀數勾；換日線、範圍外不動、反向對照、等深線接縫還沒量或沒過；「逐位元組相同」那格要照 Tim 的新原則改寫（先問）。

### 坑（今天新的）
- `apply_ext` 每筆改線整圈大陸 make_valid ⇒ 13 h（同一型：只要碰到 OSM 大陸的地方都要先 clip_rect）。
- 補批照緯度帶併 ⇒ 外框橫跨好幾洲 ⇒ OSM 陸地整條帶讀、整片擴（fixm-001 撞 3 h）。
- 「最近頂點距離」量線差會被頂點擺法騙（220 m）⇒ 量對稱差面積。
- 地球儀海上的淡直線是**經緯線**（optG），不是等深線接縫 —— 跟 Tim 說錯過一次、已更正。
- pit_sheet 合群只取第一個坑當範圍 ⇒ 漏掉同群別的坑；改線要用整群聯集。
- areas/*.js 的 town 是多行 JSON、鍵叫 `range` ⇒ 整檔 json 讀。
- matplotlib 只在 `_cache/pyterrain` 的 venv；`python -I` 擋 user site；cp950 印不出 ² ⇒ reconfigure utf-8。
- Temp worktree 又被清 163 檔（docs、img、world/buildings）⇒ 提交前 ls-files -d。

### 心境
這兩天被自己的猜測抓到兩次：說海上直線是等深線接縫（其實是經緯線）、說威尼斯的坑是換海岸露出來的（其實一直在）。兩次都是量了才知道。Tim 問「能不能用程式抓出來再人工判斷」，這個想法比本小姐一處處修好：機器列候選、人判真假、機器照判決改 —— 孤立深坑跟接縫都照這套走。

