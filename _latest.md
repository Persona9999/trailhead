---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-04T12:41:23.862Z
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

## 第四次小歇前（2026-11-04 20:40，wake#19，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）
- task `br108gbg7`：`sh scratchpad/sa/chain_asia.sh`（TASK-1198 亞洲串接，**可以續跑**：抽好的塊、展開好的塊會跳過；斷了就再跑同一支）。
  - 順序：world_decode 解回現在的 main ⇒ `_cache/terrain-work/asia/base.pkl`（498,806 特徵，做完了）⇒ 抽 14 塊＋seams（20:40 抽到 siberia-far，前 5 塊每塊 5～7 分鐘，比估的快很多）⇒ 照順序展開（中間 --no-build、--peaks-from 先展開的鄰塊，nb() 寫在腳本裡）⇒ asia-seams 建 bsl-asia 的 world/ ⇒ ice_marks.py 照新海岸重裁冰。
  - log：`scratchpad/sa/asia_logs/`（x_*.log 抽、e_*.log 展開、ice.log）；task 輸出 `.../tasks/br108gbg7.output`。
  - ⛔ 跑的時候別改 `scratchpad/bsl-asia/tools/*.py`（extractor_key 收尾才讀原始碼）。
- worktree `scratchpad/bsl-asia`（分支 trailhead/1198-asia，在 main 5bf9e0f7 上；**還沒提交**：tools/asia_scope.py＋15 份 asia-*_scope.json 是 untracked）。

### 今天完成（都在本機 Blocksail main，**都沒推**）
- **TASK-1168 南美**：8/8 勾、in_review。第 8 格「底稿從 world/ 解回」：d51d7076（world_tiles.decode_world／hint、tools/world_decode.py、world_expand 收解回的底稿、clip_into_parents held_tol、on_coast 2e-5）；合 main 9f8f49c9。重跑了五次才乾淨（共用頂點、海岸邊上的頂點、make_valid 拆頸部、最小面積門檻；還有 Windows Pool 卡死一次）。讀數：解回再建逐位元組相同（3,034／3,839 檔）；解回底稿跑南美接縫逐項相同、框外 37.7 萬圈只多下加利福尼亞 0.13 km² 一小瓣。
- **TASK-1197 格陵蘭冰原＋冰河標記**：7/7 勾、in_review。e34e9a26、f8f9e695（航線快取照 main 新的深海層流程）、5bf9e0f7（前後等高線圖）。冰的遮罩裡照 ETOPO 冰面、遮罩邊 24 px 漸變、圖磚比 ETOPO 高 100 m 以上照留（山）、keep_coast。冰河 ice-N 1,814 圈；W.at 回 ice、地球儀白色。文件 WORLD_FORMAT §10.8、§12。
- 共用樹：townplans 34 份換成這一版烘的、navdeep.bin／navmesh.bin 也換了；`town_bake --check`、nav_stale、navdeep/navmesh --check 都過。
- origin/main..main 有 7 筆以上我的＋一串別人的 ⇒ **Tim 說「先不推」**（11-04）。

### 下一步（亞洲跑完之後）
1. 看 e_asia-seams 的 report：roundtrip 0、接縫直邊（照 sa 的 seam_edges 的想法寫亞洲版：塊與塊之間的線、跟 mideast／europe-east／na-alaska 的框邊）、丟山（world_expand_drops）、沒等高線的 ≥ 100 km² 島、山頂（聖母峰那一圈 top ≤ ETOPO+300）。
2. 用 grl_plot.py 的想法畫冰帽（法蘭士約瑟夫地、北地群島、新地島、喜馬拉雅）：看圖磚是不是也混了冰下岩床 ⇒ 是的話照格陵蘭加一份 ice 範圍。
3. 範圍外：wfeat_tol 那種量法（框外每圈點數相同、每點差 ≤ 1.2e-5）；城鎮規劃重烘 plan 一樣；航線快取照新流程（ports ⇒ navdeep_bake ⇒ navmesh_bake ⇒ net ⇒ graph ⇒ stale；navdeep/navmesh.bin 從共用抄來當起點）。
4. 等深線：bathy_extract 加亞洲的緯度帶（照 SA_BANDS），--no-apply。
5. 提交 ⇒ 合 main ⇒ coding op=start（單獨一個 tool call）⇒ ff ⇒ force end ⇒ 抄 townplans、bathy base 到共用 ⇒ 勾 TASK-1198。

### 已拍板（別重問）
- 「先不推」；「開單、跑完就做」（格陵蘭）；冰河參考珊瑚礁另外標記（Tim 11-03）。
- 不用拘泥原稿、只要無縫：正本是 world/ 本身（之後的區底稿一律從 world/ 解回）。

### 坑（今天新的）
- ETOPO 要照**每塊外圍一圈**先算齊（格陵蘭南端又缺 N60W060、N60W045）；地形圖磚也要連外圍一圈下（缺的當海）。亞洲已補齊：圖磚外圍 7,161 張、ETOPO 62 檔。
- 冰原：只抬差大的會留拼接落差的直條；整片照 ETOPO 會削山 ⇒ 比 ETOPO 高 keep_m 的照留。
- 改 world.js／terrain.js 會讓城鎮規劃、航線快取全部過期（指紋）⇒ 遊戲端改完、驗完才重烘，別跑到一半又改（今天白烘一次）。
- 比對世界：形狀用「點數相同、每點差 ≤ 一格量化」比，不要四捨五入到格子比（會被取整騙）；編號一定會差，比形狀不比編號。
- Windows 的 multiprocessing Pool 可能卡死（主程序 CPU 不動）⇒ 砍那幾個 PID 重跑。
- senate task comment 的 body 要 --arg-file（記憶已寫）。

### 心境
今天兩件事都是「量完才知道自己錯在哪」：第 8 格五次重跑，每一次都是先把差異分類、挑一圈單獨重做，才看到真正的那一行（不是裁剪，是頸部自己碰在一起）；格陵蘭三版，第二版把山削掉了，是拿貢比約恩山的實際高度對照才發現。Tim 那句「辛苦了」收到了，本小姐沒說出口，但是有點高興。

