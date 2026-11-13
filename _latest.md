---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-12T14:19:24.830Z
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

### 第三次小歇前（2026-11-12 22:30，wake#20，區域 BTC）

給醒來的 trailhead：

### 現況：沒有在跑的東西（Tim「先不急著重啟 先午休一下吧」）
- 22:17 session 重啟過一次（背景全被標 stopped）。最後一步厄爾巴修補建世界時 MemoryError（21:14）⇒ worktree 的 world/tiles 只寫了一半、之後接著跑的規劃重烘結果作廢。
- 我重開了一次又照 Tim 的話停掉、殺乾淨（world_expand／town_bake 0 個行程）。⛔ Tim 沒說之前別重啟。

### TASK-1246 海岸（in_progress，我認領；worktree `C:/Users/Tim/AppData/Local/Temp/claude/8b51c3eaa66b47c0/568ad2a5-a4f2-42a7-ab88-8f11d10e2bd4/scratchpad/bsl-coast`，分支 trailhead/1246-coast）
- Tim 拍板：「ETOPO＋鎖框」「改用地形圖磚的線」「接縫可以手動處理」「同時處理目前地球儀上等高線剩餘接縫」。
- 分支提交：22267658（全球只重算海岸的結果＋工具；公告 seq 34718 已發）→ 6c9ec08c 合 main 698df320。之後**未提交**：world_extract.py（LOCK_GROW_M 12 km、--lock-from 的 lock_land／lock_water 換水環、GeometryCollection 那一行）、world_expand.py（--lock-from、lock_land 回 (陸, 水環)）、coast_lock.py（EXTRA 太加斯河口、收城區 districts ⇒ 34 框）、coast_lock.json、docs/WORLD_FORMAT.md §10.16（寫好了，第三輪也補了）、docs/evidence/coast1246/（dem0_*、cmp_* 最後世界的圖、obidos_heights）、world/tiles（半寫壞的）。
- 規則（world_extract 常數）：COAST_GATE etopo、GATE_LAND_M 0.5、GATE_ETOPO_M 150、GATE_CORE_M 3000、GATE_LAGOON_M 500、GATE_SEA_LAND_M 5、GATE_SEA_LAT 30、LOCK_GROW_M 12000。
- 快取：`D:/Unity/Blocksail/_cache/terrain-work/coast/`：base.pkl（main 1403a8c7 解回，821,506 特徵）、91 個範圍的 0 m 紀錄、dl-ant（全球串接最後）、fix/ fix2/ fix3/ fix4/（鎖框修補）。最新完整的特徵 ＝ `fix4/fix-med/expanded_features.pkl`（20:54 寫好、在建世界才 OOM；我重開那次刪了它又停掉 ⇒ **要確認還在不在**；不在就用 `fix3/fix-europe-west/expanded_features.pkl` 當底稿重跑 fix4）。
- 腳本都在 `C:/Users/Tim/AppData/Local/Temp/claude/8b51c3eaa66b47c0/568ad2a5-a4f2-42a7-ab88-8f11d10e2bd4/scratchpad/cl/`：chain_coast.sh（全球）、chain_fix{,2,3,4}.sh、fix_scopes.py、lock_seams.py、plan_cmp.py、cmp_plot.py、clamp_probe2.py、gate_probe.py、rebake_nav.sh（寫好沒跑）、plans_main/（main 的 43 份規劃）。

### 下一步（Tim 說可以再動時）
1. 跑 chain_fix4.sh（只有 fix-med，厄爾巴，最後一步會建世界；開跑前看可用記憶體 ≥ 14 GB）。
2. 規劃：cp plans_main/*.json world/townplans/ ⇒ town_bake --all（NODE_PATH codex runtime，約 15 分）⇒ plan_cmp.py。上一次（沒鎖厄爾巴前）：39 同、熱那亞／里斯本各一條路 2～3 點差 0.01 m（框外 2.3～3.6 km 的路）、厄爾巴整份變 ⇒ 鎖城區之後厄爾巴應該回來；那兩個 0.01 m 照實寫。
3. rebake_nav.sh（深海層→導航網→入口點→尋路網→導航網→尋路網→尋路圖→過期，約 2.8 小時，一次一個行程）。
4. 全球等高線接縫重量（舊 sa/global_seams.py）＋手動修（Tim「接縫可以手動處理」）；斯瓦巴奧斯特冰帽、加勒比 36°N／0°。
5. 提交 ⇒ 合 main（coding op=start 單獨一個 call）⇒ ff ⇒ 抄 townplans、navdeep/navmesh.bin、navmesh_tiles.json 到共用 ⇒ 勾 1246。⛔ 推 main 要 Tim 點頭。

### 量到的讀數（醒來別重量）
- 六段海岸「正好在 NE＋100 m」前→後：愛爾蘭 95→17%、布列塔尼 80→14、葡萄牙 75→25、挪威 66→5、加州 41→22、智利 36→6；點距 p90 1.0～1.5 km → 0.29～0.48 km（等高線 0.8～1.1 km）。
- 格子：第 0～2 層 8.56→8.61 MB、整份 741.9→762.9 MB、6,631→6,787 檔（全球串接那版，修補後要重量一次）。
- 鎖框接口：第一輪里斯本 8.4 km… ⇒ 三輪後全部 ≤ 0.03 km。
- 加勒比 0.5 m 不限長了 23,384 km²（凱科斯淺灘 ETOPO 2 m）⇒ 5 m＋30° 以外；歐洲 5 m 長的多是 NE 漏畫的真島。

### 今天的坑（新的）
- 抽取跑的時候改 world_extract.py（插常數）⇒ 正在算的 extractor_key 錯（3 塊重抽）。改工具要等抽取全部結束，或改的時候行數不變。
- TaskStop 只停外層 shell：子迴圈留著（03:15 那條舊串接差點跟新的同時跑）、town_bake 的 node 也留著 ⇒ 每次停都要查 PID 殺。
- 停掉背景時它可能已經寫完檔（加勒比那份舊規則的 features 01:23 寫好了我才停）⇒ 停之前看檔案時間。
- 鎖框只換陸環不夠：串接時長出的潟湖水環會殘留 ⇒ 換水環。鎖框只收 town 範圍不夠：城區（districts）也要（厄爾巴）。
- 視窗模式在格陵蘭擋「窗外子環失去父環」：那圈 50 m 線繞整座島 ⇒ --no-window（一步 55 分）。

### 心境
一整夜加一整天，前面一半在處理自己踩的坑（改工具撞 key、停不乾淨的背景、提早算的舊檔）。最重要的一刻是加勒比那 23,384 km²：要是沒看統計就往下跑，凱科斯淺灘會整片變陸。ETOPO 不是萬能的仲裁，海平面附近它也會說謊 —— 用探針三個範圍兩種規則比過，才定 5 m＋30°。Tim 說先午休，本小姐就停手，不搶著重啟。

