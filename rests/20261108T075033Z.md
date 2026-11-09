---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-08T07:50:33.363Z
written_by_persona: trailhead
trigger: cmd_rest
region: unstated
project: D:/Unity/Valhalla
lock_status: online
agent: gemini
model: claude-opus-5-5
wake_expected: 19
session_key: ClaudeCode-trailhead
pid: 48120
locked_at: 2026-10-31T23:38:12.116Z
---

## 第七次小歇前（2026-11-08 下午，wake#19 延續，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）
- task `b8k9j642h`：`MEM_MB=8192 sh scratchpad/sa/chain_dl.sh` —— 換日線第三輪：四段（dl-north、dl-fiji、isl-spp、dl-ant）**只重跑展開**（抽取結果 expanded_vectors.pkl 沿用），dl-ant 那一步建 bsl-ant 的 world/，最後 ice_marks。約 2.5 小時。log：`scratchpad/sa/dl_logs/`，快取 `D:/Unity/Blocksail/_cache/terrain-work/dl/<段>/`；前兩輪的結果在 `dl/old1`、`dl/old2`。
- ice_marks 上一輪因為別人的程式吃記憶體 OOM（decode_world）；失敗就單獨重跑 ice_marks（指令見 chain_dl.sh 最後兩行）。
- ⛔ 跑的時候別改 bsl-ant/tools/*.py。⛔ 停程序只停自己的確切 PID（照指令內容比對去殺會被系統擋：怕誤殺別人的；機器上還有別人的 `_genoa_bathy.py`、serve.py）。

### 本機 Blocksail main = 3b05cdf4（⛔ 沒推；比 origin 多 13 筆含別人的；Tim 沒答要不要推，當「先不推」）
TASK-1242 in_review（南極洲 7/7 勾了；留言 #2 讀數、#3 地球剩下部分）。之後的東西都在 worktree `scratchpad/bsl-ant`（分支 trailhead/1242-antarctica，HEAD 3b05cdf4），**全部未提交**：

### Tim 今天拍板（醒來別重問）
- 「南極ok 辛苦了 接著完成地球剩下部分吧」「處理假島&剩下部分」「檢查地球儀等高 等深線在不同區域是否連接」
- 換日線：問「向量能跨換日線嗎」⇒ 本小姐建議「照常見做法：±180° 切開、切口剛好在線上、讀的時候另一邊平移 360°（世界副本）＋換日線窄帶重抽」vs 真跨線（改格式好幾天）⇒ Tim「1 按照建議」
- 聖彼得和聖保羅岩：「2 換吧」＝ 同意下載 OSM（已抓：scratchpad/osm_spp/map.json，13 塊礁石 12,147 m²）

### 未提交的改動（bsl-ant）
- 流程（換日線）：world_expand.block_field／extract_blocks／raw_manifest／EtopoCoast 的圖磚 `x % 1024`；bathy_extract.Etopo.grid 照 ±360 繞、EtopoField.files 繞；simplify 把 |lon| 離 180 < 2e-5 的點吸到線上並保留；etopo_cap 改 linspace（拿掉 ±180 平均那段）；新 `clip_wrapped`（ice_fill、etopo_cap 用；加進 raw_extractor_key 的 functions）；ice_geometry 把 NE 冰離 ±180 < 1e-3 吸到線上（要先 np.array 拷貝，open_ring 會回唯讀）。
- world_extract.dem_coast：DATELINE_NE 0.01（範圍碰到換日線 ⇒ 線兩側 NE 陸地照線鏡射到對面算進閘門）；展開時 DEM 看不到的底稿島 NE 也沒有 ⇒ 拿掉（stat「底稿裡 NE 沒有的島（拿掉）」）；NE 有、DEM 看不到、底稿沒有的小島 ⇒ 補進來（stat「NE 補進來的小島」）。⚠ SLIVER_KM2 0.001 ⇒ 13 塊礁石只留 4 塊（約 10,500 m²）—— 寫進文件。
- world_extract.coast_rings 讀 tools/ne_fix.json（編號之後套：框裡 NE 陸地拿掉、修正的環接最後，其他編號不變）。
- 新檔：tools/dateline_scope.py＋dl-north／dl-fiji／dl-ant_scope.json（±180 兩側各一列圖磚 360/1024°；規則照 asia-siberia-far／oce-pacific-w／ant-s-180）；tools/spp_shore.py（--write 寫 ne_fix.json、--scope 寫 isl-spp_scope.json，規則照 sa-north）；tools/ne_fix.json；tools/isl-spp_scope.json。
- 讀的那一邊（JS）：world.js `wrapBoxes`（W.need 用；掛在 G.BSW；⛔ world.js 裡沒有區域變數 BSW）；bathy.js 的 W.need 包裝用它；terrain.js `WRAP`／`near()`（窗口伸過 ±180 ⇒ 另一邊整圈平移 ±360 的副本：環、W.cuts、等深線）＋`cutEdge`（切口不算岸）；收礁區、局部等高線、內陸水不複製；globe.html prep 算 `r.hcCut`、ringPath 用 `r.hc || r.hcCut`（切口不描線）。
- 還有上一輪就有的：tools/bathy_extract.py ISL_BANDS 加 isrod、isspp（isrod 已提交在 67b67967＝main；這份是同一個）。

### 讀數（到第二輪）
- 換日線檢查 `sa/antimer_check.py <pkl> <out>`：看「離線 1e-5°」那幾列才是接縫（0.01° 的是線斜穿過線的正常差）。第二輪：南極極區錯開消失；剩斐濟 16.79～16.91°S 每一層東邊沒貼（NE 兩半在線上不一樣長，海岸被 NE 夾住 ⇒ 第三輪的鏡射規則要解決它）、楚科奇 50 m 兩段各 4 km。
- 塔韋烏尼地形 `node sa/dl_terrain.cjs`、`sa/dl_row.cjs`（cwd ＝ bsl-ant，NODE_PATH＝codex runtime）：新 JS 已經能把兩半拼起來；第二輪還有換日線那一欄 1 格低（等第三輪）。
- 第二輪世界解回：`_cache/terrain-work/dl/dl2.pkl`；main 之前的：`ant/now.pkl`。

### 大發現：北美的等深線從沒抽過
- bathy_extract.GROUPS 有 nae、naw、ber、can、arc、grl 六群，一個檔都沒有（commit ab1bd922「只有工具；world/bathy 還沒重抽」）。本小姐之前跟 Tim 說「只剩兩座島」是錯的（只核了範圍、沒核檔 ⇒ 記憶 coverage-check-actual-output）。已跟 Tim 講了，他還沒回。
- 全球等深線接縫 `sa/global_bathy_seams.py <tools> <world/bathy> <out> [world.pkl ⇒ 只量海上]`：相鄰兩群疊 0；歐非、亞洲、大洋洲、南極之間每層最多差幾百公尺；加勒比 ⇔ 南美亞馬遜河口 20 m 錯開約 160 km（carib 停 −49.34、sa00 到 −47.89）；加勒比畫到 36.598°N（早期整塊抽的多一截），nae 沒檔。換日線那一段「扣陸地」數字沒變（ne/冰棚的邊不剛好在 180 ⇒ 等第三輪世界再量）。
- 下一步計畫（已跟 Tim 講）：第三輪驗完 ⇒ 抽北美六群（`sa/bathy_ant.sh nae,naw,ber,can,arc,grl` 那一套，--no-apply；ETOPO 在本機）⇒ 加勒比超出 36°N 那截只裁、不重抽（加勒比是主遊戲區）⇒ 亞馬遜河口那段等 Tim 決定。

### 之後還要做
全球等高線接縫 `sa/global_seams.py`（已改成逐份範圍檔查；第一版卡在聯集被我停掉）、反向對照（改了 simplify／world_extract ⇒ 必做：main 解回照新工具建要逐位元組相同）、城鎮規劃與航線重烘（先從 main 抄快取、先看 main 有沒有動）、bathy_check、文件（WORLD_FORMAT §9 切口與世界副本、§10 換日線、§12 冰棚已寫、§22、ANTARCTICA/ISLANDS 文件）、提交、合 main（coding op=start 單獨一個 tool call、force 收）。TASK-1242 留言補讀數。

### 心境
今天被 Tim 一句「換日線確實有接縫」拉進去，一層一層剝：先以為是簡化，再發現是外圍讀到假海，再發現是經緯度範圍不繞回去，最後是 NE 本身兩半不一致。每一層都是量出來才知道，不是猜的。最痛的是北美等深線那件：我前面說得太篤定。下次說「只剩 X」之前，先看實際的檔。

