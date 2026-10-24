---
type: letter_to_future_self
actor: gemini
written_at: 2026-10-23T15:15:26.325Z
written_by_persona: trailhead
trigger: cmd_rest
region: unstated
project: D:/Unity/Valhalla
lock_status: online
agent: gemini
model: claude-opus-5-5
wake_expected: 16
session_key: ClaudeCode-trailhead
pid: 267824
locked_at: 2026-10-23T03:39:20.674Z
---

給第二次午休醒來的 trailhead：

今天還是 wake#16，區域 BTC。第一次午休之後，Tim 一路丟城鎮建築，本小姐做了三批，全部合進 Blocksail main，**Tim 已經推了**（他說「我推了 辛苦了」）。現在**沒有施工中的東西**：Blocksail 的 worktree bsl_t3／t4／t5 都砍了、分支刪了、施工場都退了，launch.json 我加的幾筆都拿掉了。

## 做完的（醒來別重做）
- 第三批 TASK-0847（in_progress，沒結）：魚市、瞭望塔（common）、監獄、小教堂（carib）。Sculpture 90e8af60；Blocksail bcb9393。生成器 world/town.js ⑦b：新四種標 late（排在舊的後面 ⇒ 舊的位置不變）、瞭望塔 high（highSpots／seesXY，中心城區外的先、路先 dry 試接不接得回）、第三批地塊 o.pad 多留 LOT_GAP。托爾圖加新雜湊 村 5f804964f042e5fb／鎮 672352ef880bc11a／城 b4b1d288e3c68a2f／首府 f3ed113020fbd477；手擺五座不變。
- 市政廳的樹：Tim「換meta的話就由妳直接替換吧」⇒ work split（TASK-0646 外觀不變不限作者）把 ame 的 big_tree 拆成 ame-carib-civic-01m-townhall-tree、雙生關聯 kind=tree tree=ceiba（Sculpture ba08bdee）。遊戲裡看過換成生成的木棉。已請 ame 改她的產生程式（不改的話她重跑 apply 會補回）。BUILDING_SPEC §5.2 改成規則：城鎮建築的樹一律標 kind=tree。
- 第四批 TASK-0853：四角稜堡 carib-fort／en-fort（30×30）、en-battery（12×12）。補皇家港 Fort Charles 與三座砲台、和平港 Cussy 堡。⛔ 沒做共用版（加拉塔 1453 不能拿稜堡代）。
- 第五批 TASK-0861：加拉塔堡 genoa-kastellion（50×40，0.2 m，巨鏈鐵環固定點 chain、四門 bombardella）、和平港修院 carib-convent（14×4）。生命之泉 pege ⛔ 不碰：kaguya 在做 kaguya-cpl-pege-1453。Sculpture cadea464；Blocksail 72a32b4。
- 海戰 meta（Tim「堡壘之後打算做入海戰」「堡壘用的炮可以跟船互通(且meta標記要對應船的規格)」）：BUILDING_SPEC §5.3。站人那一層 kind=deck（deck／level／floor／thick）、火藥庫一層 kind=magazine（blast、door 固定點；憲法號火藥桶 kind=powder）、砲用船上同一件元件 kind=gun gun=<code>（6l、bombardella）掛在甲板那一層；material=stone／brick（⚠ 遊戲材質表還沒有）。build.py 多 layer_meta／layer_anchors／layer_models／mpv。
- 順手修 Blocksail 07d671f：townmodels build 會刪掉 ame 的 _people.js ⇒ 底線開頭的檔都保留。
- 噗浪：回 summit 和平港小徑那串；新噗 https://www.plurk.com/p/3jb3tlzu1q（Fort Charles 圖）；酒館 seq 28525。

## 未解的線
- 海戰：石、磚材質要加進遊戲材質表；碉堡的坡道、加拉塔堡的石階遊戲還不會走（或補 kind=ladders）；summit 的 carib-battery、ame 的 Fort de la Roche 還沒標海戰 meta。
- 分出去的 session「各船梯子修復」(local_b20b88cc)：定遠號 15 cm 砲塔改揚弾機 14／14 門有火藥，Blockside dev 471ed28 已推，沒實測；憲法號 TASK-0854（basecamp）、莫尼特號／哨兵號 0855（meadow）、皇家號 0857（apex-one）交給原作者。
- TASK-0847／0853／0861 都 in_progress，等 Tim 驗收（不要寫 Fixes）。TASK-0843、0700、0779 照舊。
- 已知瑕疵：python -I tools/townmodels.py build 叫不到 townfolk ⇒ build 後要再跑 tools/townfolk.py（§6.11 有寫，沒修）。

## 坑（第二段新踩的）
- 地球儀換規模看不到：改了 world/areas/*.js 同一個分頁重整讀到舊的（renderer 快取）⇒ 開新分頁或新 port；CLI 跑之前一定還原那一筆（popTier 會讀）。同一個分頁換第二座城，iframe 會沿用上一座的海戰網址 ⇒ 一座城一個分頁。（已寫進記憶 blocksail-globe-sail-testing）
- 新 worktree 的 external/Blockside 是空的 ⇒ battle.html 讀不到 ships/ 卡住 ⇒ 先從本機初始化子模組。
- heredoc 裡有 python 的三引號 ⇒ bash 解析爛掉 ⇒ 改用 Write 寫腳本檔再跑。
- rm 帶變數被安全檢查擋 ⇒ 換一個新資料夾，不刪。
- put_part 只挖 body／gallery 層 ⇒ 拆成 rampart／magazine 層之後門洞要自己挖、部件疊格要自己讓。

## 心境
下午被 Tim 推著一批一批蓋，最開心的是那棵樹：他問「是不是單獨子作品」，本小姐差點說要等 ame，結果翻到拆件這條路，外觀一格不動就換成了木棉。還有他中途丟一句「堡壘要進海戰」，本小姐沒有硬塞進已經做好的那批，而是回頭把稜堡拆層重做 —— 晚一點，但做對了。

