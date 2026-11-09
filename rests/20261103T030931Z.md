---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-03T03:09:31.530Z
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

## 第三次小歇前（2026-11-03 11:10，wake#19，區域 BTC）

給醒來的 trailhead：

### ⚠ 背景還在跑（compact 不會停它）
- task `b8iz10kor`：worktree `C:/Users/Tim/AppData/Local/Temp/claude/D--Unity-Senate/1bebeeb8-61c3-46ca-a41f-778b0f55c620/scratchpad/bsl-sa`（分支 trailhead/1168-sa，HEAD 92620e0e）上依序：nav_ports（完成）→ nav_net → nav_bake（整份重烘 graph，舊的抄在 scratchpad/sa/graph_before3.js）→ nav_stale → 從共用 main 抄規劃到 sa/plans_main3 → town_bake → --check → cmp_plans。輸出檔 `.../tasks/b8iz10kor.output`；各步 log 在 scratchpad/sa/nav_*3.log、bake3*.log。
- ⛔ 它在跑的時候別改那個 worktree 的 tools/（記憶 terrain-key-edit-during-run）。
- 預覽伺服器 trailhead-sa（port 8841，launch.json 裡我加的那一筆，只刪自己那筆）還開著。

### TASK-1168 南美洲等高線＋等深線（in_progress，勾了 4/8：等高線、山頂、等深線、反向對照）
- 分支提交：864d9301（南美全部：tiles 1,397、bathy sa*.js 131、index、tools/sa_scope.py＋3 份範圍檔、bathy_extract.py、docs SOUTH_AMERICA_TERRAIN.md＋WORLD_FORMAT §10.7＋evidence 4 張截圖）→ b9a53e3b（合 main 379b20a4）→ 92620e0e（合 main b2664430）。兩次合併都只有 world/bathy/index.js 衝突：取 MERGE_HEAD 那份＋接上 sa 的 131 個 files 項目和 8 個 src，用 tools/bathy_extract.write_index 寫回（腳本寫在對話裡；照做一次即可），bathy_check 都過。合併要先 submodule update --init（本機 url，記憶 blocksail-worktree-submodule-hook）否則國旗 hook 擋。
- **下一步**：等 b8iz10kor 跑完 ⇒ 提交 world/nav（ports/net/graph；commit 寫明：合併前只有南美時 graph --check 過，合併後 graph 變的是 main 那一串：多 1 條坎佩切｜查爾斯鎮、改 12 條加勒比／大西洋／地中海，沒有經過南美；ports/net 只換 src 雜湊與 fp）⇒ 若 main 又動到 world/bathy|nav|terrain.js|town_store.js 再合一次 ⇒ coding op=start（單獨一個 tool call，scope 絕對路徑用 | 分隔：D:/Unity/Blocksail/world/tiles|world/index.js|world/bathy|world/nav|tools/sa_scope.py|tools/sa-*_scope.json|tools/bathy_extract.py|docs/...）⇒ ff main ⇒ force end ⇒ 抄 bsl-sa/_cache/bathy/base/sa*.js 到共用 D:/Unity/Blocksail/_cache/bathy/base、抄 bsl-sa/world/townplans/*.json 到共用 world/townplans 再 town_bake --check ⇒ 問 Tim 推不推（⛔ 沒點頭不推）。
- 還沒做的格：範圍外不動（城鎮規劃、航線快取那部分 —— 規劃內容 30 份一樣已量過一次）、地球儀截圖（做了；那一格還寫了「航線快取照新的地形重烘」⇒ nav 提交後再勾）、文件與致謝（做了，可以勾）、**底稿丟了也能接（第 8 格，從 world/ 解回底稿 ⇒ 建出來跟 main 逐位元組同、拿它重跑南美比；還沒動）**。
- 串接最後一份：`D:/Unity/Blocksail/_cache/terrain-work/sa/sa-seams/expanded_features.pkl`（747 MB，sha256 fe7ec5be65e4e068…）；請 Tim 備份（說過了）。

### 已拍板（別重問）
- 底稿從另一台搬來（Tim 傳了 carib、greenland 兩個 zip，已解到 _cache/terrain-work）；「可以直接在這台重新下載」；「工具規定可以改」「只要能無縫連接就好」「很多地形 等深線都是手改過的」「不用拘泥於原稿」⇒ 第 8 格。
- 加勒比等深線赤道切邊（carib-38/91/93 離 0° 0～1 km，main 本來就有）⇒ Tim 選「先記缺口另開單」⇒ **TASK-1189**（bug，related 1168，我開的、沒認領）。
- Blocksail main 還有一個推不推的舊問題：本機 main 上有別人的提交、Blockside dev 的 9ea8e4b、25eb85e 沒在遠端 ⇒ Tim 沒回答，⛔ 沒推。

### 今天坑（新的）
- 接縫窄帶：前一步已展開的接縫要**跨兩側**（只抽一側 ⇒ 625 km 兩套線；na_scope 的 conform 就是為這個）。
- 抽取中途改 bathy_extract.py ⇒ sa-south 的 extractor_key 壞掉（inspect.getsource 讀偏）；已寫記憶 terrain-key-edit-during-run。
- 另一台的 carib 紀錄在這台 key 對不上（推測環境）⇒ 這台重抽 terrain-work/carib-local 只拿最高點。
- ETOPO 缺檔跑到才知道（N15W030、S60W030）；之後先一次算齊。
- 等深線量法：容差要 > 1e-5（檔案精度），不合法的圈用 make_valid（buffer(0) 會砍掉一片）；Etopo.sample(xs, ys) 回傳整張格子不是逐點（我量錯一次說卡多索島 621 m）。
- 改工具前先看完「有沒有在跑」的結果再動手（看到 count=11 沒等就改，幸好是別人的）。

### 心境
這一輪本小姐錯了好幾次：窄帶切錯邊、改檔弄壞 key、取樣寫錯、量法誇大。每一次都是「先量、畫圖對照、找到真正的那一行」才翻過來。最值得留的是接縫那件：數字說 625 km，本小姐沒有先怪工具，而是去讀北美的 conform 為什麼存在 —— 前人已經把這個坑寫在程式裡了，只是我沒讀。Tim 今天兩句「不用拘泥於原稿」「只要能無縫連接就好」，讓我把「正本」從快取的底稿換成成品本身。

