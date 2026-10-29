---
type: letter_to_future_self
actor: gemini
written_at: 2026-10-26T10:49:10.600Z
written_by_persona: trailhead
trigger: cmd_rest
region: unstated
project: D:/Unity/Valhalla
lock_status: online
agent: gemini
model: claude-opus-5-5
wake_expected: 17
session_key: ClaudeCode-trailhead
pid: 69208
locked_at: 2026-10-26T07:36:54.427Z
---

給午休醒來的 trailhead：

今天是 wake#17，區域 BTC。早上 Tim 給了自由意志模式，下午一連串都在 Blocksail 的生產與市場上。

### 已經落地、推上去的（醒來別重做）
- 和平港 Cussy 堡門前的石階 ＋ 生成器 roadStairs 往回併直線頂點：Blocksail 286c603、合併 839d11a，Tim 說推 ⇒ 推了。
  Blockside dev 先推到 5b38213（main 的子模組指標指著它，遠端原本沒有；Tim 選「先推 Blockside dev 再推」），Blocksail main 推到 eee94b5。
  推的時候 Sirius 剛合進三筆（9fa2810、7e408fc、eee94b5）一起上去了，酒館 seq 30071 照會過。
- 噗浪：回 summit 和平港小徑串（附石階截圖 C:/Users/Tim/AppData/Local/Temp/claude/D--Unity-Senate/ceac69e8-0cda-472f-be26-c8fa10071f3c/scratchpad/pdp_perron.jpg）、回 gura 換配方串、新噗 359182677295403「再便宜的肉，也沒人吃」；酒館 seq 30100。

### 施工中：分支 trailhead/byproducts，**還沒合進 main**
- worktree：C:/Users/Tim/AppData/Local/Temp/claude/D--Unity-Senate/ceac69e8-0cda-472f-be26-c8fa10071f3c/scratchpad/bsl-bp（裡面有自己的 _cache/townsample 副本；townmesh 沒有 junction）
- 6 筆：f5f3f16 第 18 批四種加工品登錄（鞋、銅器、蜜餞、陶菸斗；TASK-0992）／9e9d459 養雞副產品（已被下一筆取代）／968e87e 牲口與屠宰場（TASK-0991）／4b25d28 合 main a334bd9／57c7311 吃的花樣 CONSUME.variety 0.25／17d12c9 牧場 牛 0.05＋牛奶 0.2、牛 418.1、牛奶 15.9
- worktree 的 world/townplans 九座在 17d12c9 之後重烘過、--check 全 ✓ ⇒ 合併時照早上的流程：coding op=start **單獨一步** ⇒ ff ⇒ 抄 townplans 回 D:/Unity/Blocksail/world/townplans ⇒ --check ⇒ town_start 讀一次 ⇒ op=end force=1。
  ⚠ 合之前先看 main 動了沒（今天下午 main 動得很快：gura 的價格曲線 4b6b23d、kiara caridad 都是我做的時候進來的）；動了就在 worktree 合 main、重量、重烘。
- **等 Tim 拍板合不合**（最後一個 wrap_up 問的）；他上一句是「辛苦了 /scp-plurk」，沒回這題。

### 已拍板（別重問）
- 架構本來就支援主產品＋副產品（Tim「看起來已經支援了」）。
- 牲口鏈照 Tim 的設計：養雞場 雞蛋＋活雞、牧場 活牛＋牛奶、屠宰場 活雞 ⇒ 肉＋羽毛、活牛 ⇒ 肉＋牛皮＋牛油；豬圈照舊（Tim 沒提）。
- 牧場「牛奶比例多一些 牛少一些」⇒ 選了 0.05＋0.2（0.04＋0.15 也試過）。

### 量到的、別重量的
- 副產品缺貨漲價會撐著開工（mktRate 照全部產出市值）：鋸木加木柴 ⇒ 木板堆 1,800；伐木加木柴 ⇒ 亞拉斯原木一直爬；屠宰場因為牛皮佔七成市值照開。副產品照基礎價封頂能擋，但會讓肉滿貨時豬圈停、肥皂沒牛油（processed_goods_check 不過）⇒ prod.js 沒改，寫在 PRODUCTION §1.2。
- 肉沒人吃的根因：2 級菜只挑最便宜的一道吃滿上限（聖多明哥全是海產），肉價跌到一成還是理論消耗 0 ⇒ 「比主糧便宜就補進主糧」從來沒觸發、拿掉；variety 0.25 有效（0.4 不夠）。
- 還堆的：聖多明哥牛皮（main 上就堆；皮革沒有下游 ⇒ 等 TASK-0992 鞋匠鋪）；chain_check 砍柴伐木場在 100 上下跳（main 101 也是 ✗）。
- 有一輪聖多明哥麵包（木薯餅）兩年 3,700 桶：新價格曲線要跌到三分之一烘焙坊才收手 —— 是曲線的事，不是牧場。
- 工具：scratchpad 的 probe_glut.cjs（某城某貨每 60 天的庫存、天份、價）、cmp_stats.cjs（G=貨,貨 環境變數）、run_prod2.sh（WT=worktree 名 標籤）。

### 單子
- TASK-0991（牲口與屠宰場）in_progress，留言 #1 寫了「球在 Tim：合不合」；TASK-0992 in_progress（只做了登錄，配方、建築、消費還沒）；TASK-0993 美術第 18 批 8 張（掛 0880 下），等美術。
- TASK-0861：留言寫了樓梯讀數，還是等 Tim 遊戲內確認。

### 心境
今天最想留的是那個「再便宜也沒人吃」。本小姐一開始以為是降價不夠，想讓市場「加速消耗便宜的貨」；一量才發現理論消耗是 0 —— 不是吃得慢，是根本沒人點那道菜。數字先講話，再決定要修哪裡，這個順序又救了本小姐一次。還有：中途兩次差點把「量出來不對」的東西合進 main（肉堆、木板堆），都是停下來先問 Tim。停下來不丟臉，硬合才丟臉。

