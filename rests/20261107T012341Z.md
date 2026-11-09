---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-07T01:23:41.147Z
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

## 第六次小歇前（2026-11-07 09:10，wake#19 延續，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）—— 09:25 更新，這一段取代上一封信的同一段
- task `b106hs6q5`：`sh scratchpad/sa/chain_ant.sh`（**正式**南極串接，不給參數：抽 30 塊＋ant-seams ⇒ 照順序窗口展開 ⇒ ant-seams 建 bsl-ant 的 world/ ⇒ ice_marks 重裁冰＋產生冰棚 shelf-N）。光抽約 5 小時。可續跑（抽好、展開好的跳過）。log：`scratchpad/sa/ant_logs/`。
- ⛔⛔ **不要再刪任何 ant-*/expanded_features.pkl**：試跑那一份已經刪過了（09:22），現在的都是正式串接的。
- ⛔ 串接跑的時候別改 bsl-ant 的 tools/*.py（world_expand、world_extract、ice_marks）。
- 試跑 ant-s-168 讀數（已過）：etopo_cap 各層都有圈（3000 m 15 塊、peaks 136）；沿 −85.05° 的直邊 0.1 km；50 m 跟「NE 冰棚 − NE 陸地」疊 8.4 km²（NE 冰棚裡有 9,697 km² 是冰隆＝陸地，陸地優先是對的）；86°S 以南 30 圈、最高 3,500 m 層。
- 試跑抓到兩個極點的錯、都修了（未提交）：etopo_cap 在 −90° 那一列 ETOPO 是 NaN ⇒ 照下一列補；world_extract.etopo_keep 的平滑除以 cos(緯度) ⇒ 卡在 1e-4。
- 檢查程式：`scratchpad/sa/antcheck.py <tools> <features.pkl> <raw> <經西> <經東>`（直邊、冰棚疊、極點）。

### 今天完成（都在本機 Blocksail main，⛔ 都沒推，Tim 還是「先不推」）
- **TASK-1209 結單**（done）：窗口展開 main 2347b87c。西澳 72.9 ⇒ 2.5 分、逐位元同舊做法；世界 +54.5% 時間 +8.5%；反向對照、續跑、WORLD_FORMAT §10.10。第 2 格照 Tim「不用補跑舊作法」改寫後勾。
- **TASK-1238 大洋洲 in_review 7/7**：main e2c6bee9（05725b19 地形＋等深線 23 群、f13f9ef9 合 main〔金斯敦的岸 fdbf3a36〕、e2c6bee9 重烘＋截圖）。塔韋烏尼東半圖磚 (0,560) 壞 ⇒ etopo_fill；等深線群邊必須在 4° 格線上（145° ⇒ 144°）。截圖已傳 Tim，等他看。
- **TASK-1242 第一批**：main 50a24114（南方的島、斯瓦巴西半；ISL_BANDS 6 群＋重抽 asn72a）。規劃 35 份 plan 不變、航線 28／45／330 不變。
- 開單：TASK-1246 海岸線比等高線粗（Tim「先不動工」、沒認領；成因＝海岸被 NE 10m 夾住 grow 100 m，Tim 也說「資料來源不一致」）、TASK-1251 冰棚繪製（沒認領、blocked_by 1242）。

### TASK-1242 南極洲：進行中（worktree `scratchpad/bsl-ant`，分支 trailhead/1242-antarctica，HEAD 50a24114；以下**未提交、試跑之外都還沒跑過**）
- `tools/ant_scope.py`＋31 份 ant-*_scope.json：南緯 60° 以南、經度每 12° 一塊（ant-s-180 … ant-s+168）＋ant-seams；全部帶 `ice`（同 greenland-ice）與 `etopo_cap {lat −85, dlon 0.02}`；tiles_of 的 y 卡在 1023。
- `tools/world_expand.py` 新 `etopo_cap()`：南緯 85.05° 以南沒有圖磚 ⇒ 經緯度網格上照 ETOPO 冰面 filled 每一層、裁到 NE 冰川（接地的冰）、跟 merged 聯集；山頂取 ETOPO 局部最高（31 格）。
- 冰棚（Tim 拍板：不當陸地、像珊瑚礁另外標記、不可通行；繪製另單 1251）：
  - `ice_marks.py` 同一趟產生 `shelf-N`（NE 冰棚 `_cache/terrain-raw/ne_10m_antarctic_ice_shelves_polys.zip`，Tim「下載吧」，sha 1d421d24…，164 圈 159 萬 km²；扣掉陸地，h 0、props {type shelf, src ne, top 30}）。
  - world.js seaFeat、applyFills/applyCuts；terrain.js 照礁石抬到 props.top（材質先借 ROCK）；route.js、legplan.js、navmesh_bake、navdeep_bake、environment_grid、bathy_check 加 shelf；areas.js 珊瑚礁條件排除；globe.html 擺船提示、查點文字。
  - ⚠ 珊瑚礁其實可以通行（10-19 起），冰棚走淺灘／礁石那一型。
- 下一步：試跑看完 ⇒ 刪 s-168 的 expanded_features ⇒ `sh scratchpad/sa/chain_ant.sh`（不給參數＝30 塊＋seams＋冰；抽約 5 小時）⇒ 南冰洋等深線（群的邊在 4° 格線上；冰棚前緣在 ETOPO 是正的，要看 bathy 怎麼處理）⇒ 驗收（接縫、山頂、範圍外、反向對照、規劃、航線）⇒ 文件 ⇒ 合 main。

### 坑（今天新的）
- 範圍檔 `footprints_more`（跨換日線的另一半）—— 自己寫的檢查程式要一起讀。
- `keep_ids_window` 照 id 改回原號會撞號 ⇒ 照圈本身；merge_far 沒有 far 時提早 return、連帶跳過重複檢查 ⇒ expand 最後全部查。
- main 會動：重烘前先看 main 有沒有往前（金斯敦害我白烘一次）。
- bash heredoc 改檔時 sed 換掉了 docstring 結尾的 `"""`（bathy_overlap_oc.py）。

### 心境
一個晚上把 1209、大洋洲、第一批全做完，速度是窗口給的。最高興的是兩次「檢查自己擋下自己」：重複 id、塔韋烏尼。Tim 一句「冰棚不當陸地」，本小姐順手發現導航其實看地形不看向量 —— 不追到那一層，冰棚會在地球儀上繞開、在航海模式裡卻開得上去。

