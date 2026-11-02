---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-01T15:47:34.969Z
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

## 第二次小歇前（2026-11-01 23:20，wake#19，區域 BTC）

給醒來的 trailhead：

現在**沒有施工場、沒有我開的 worktree、沒有背景量測**。我的提交全推了：Blocksail origin/main 70b8a2b5、Blockside dev 697a27f。本地 main 上的 a2503eae（四級食物框）、Blockside heads/dev 的 9ea8e4b、Sculpture 的 4f968076／b82f9d1c 都是別人的，⛔ 不是我的、別推。舊 worktree `ceac69e8…/scratchpad/bsl-levels`（trailhead/levels，早被取代）還在，⛔ 別砍、也別用。

### 午休後做完的
1. **TASK-1144 小飯館** 結單：Tim「小酒館ok 推上去吧」⇒ Blocksail e752f41b、Sculpture de5b0159 推了。
2. **TASK-0998 餐廳** 結單：遊戲裡托爾圖加蓋得出、點到面板寫「餐廳　主人：玩家」；順手修 service_bld_check 抽不到 tbiRcpHTML（ccea82b5，被百夫長號那次推出去）。
3. **崗位名稱 14 種**＋**菜單改每名工人**（顯示）：Blocksail 96d6974a＋接指標 6afb5d6b；Blockside d79eee8。廚師、酒館夥計、侍者、客棧夥計、醫護、修士、教師、工匠、稅吏、港務員、商行書記、市府書記、過磅員。
4. **TASK-1154 每名工人格式＋混合菜單**（in_progress，5 格勾 4）：ee1f82c1、6091ab0e、b19f83bd、e3f34bc9 ⇒ 推到 fb1f48d2；再「滿編歸建築崗位」a47c9ffd、03523935、接指標 70b8a2b5（Blockside c45a7fd、15c6f50、697a27f）。
   - SERVE_RECIPES 是 per（每名一天招待）＋ in（每名一天吃）；建築 crew；BSW.serveCrew；BSW.SERVE_MIX（false ⇒ 一天一道）。
   - 挑菜照「每名工人給的滿意度」sat × per（⛔ 照整道 q×sat ⇒ 彌撒 61 人輸給禮拜 200 人，整天禮拜）。
   - 面板：崗位那一行 ＝ 滿編（crew × mult）；菜單那一行只寫「每名 X 一天招待 N 人次・昨天 M 人次」，⛔ 不寫滿編、每日最多（Tim：之後有十人的大餐館、聖索菲亞會招待更多）。
   - ⚠ 第 4 格沒勾：一年裡吃的那一本人次 −1～3%（想來的少了，錢花在喝的、捐獻），滿足率反而升；餐廳招待 −30%（酒館接走）、巧克力館 −21%（酒館多賣巧克力杯）。單子別結。
5. **稅的反向對照**（auto_check ⑧）：沒稅那邊以前只關 income、asset，階梯稅 famSteps、folkSteps 兩邊照收 ⇒ 正負跟雜訊走。關掉後 main 4.73M ＞ 1.36M。
6. 推 fb1f48d2 前合了 origin 的北美地形整份重跑 ⇒ 本機 23 份規劃全部重烘（--check 過）。規劃不進 git。
7. 噗浪：回 summit 卡拉維爾（359234381027996）、新噗混合菜單 https://www.plurk.com/p/3jcdpv1f9g 。

### 已拍板（醒來別重問）
- 工人數照「最常做那一道的人數」：教堂、修道院、酒館、餐廳、巧克力館 3，醫院、學校、小飯館、客棧 2。
- **接受新容量**：同一座裡別的菜容量跟著變（禮拜 200 ⇒ 300、葡萄酒／聚會 100 ⇒ 150、宴席 70 ⇒ 42）。
- 滿編寫在建築崗位，不寫在菜單。

### 未解的線
- summit：卡拉維爾要等豐沙爾、安格拉（TASK-1090 等 Tim GO）；蓋的時候連船廠一起看，港裡真的下水一艘。
- basecamp 起重架吊臂、ame 補丁木材、TASK-1106（球在 Tim）照舊。

### 今天新踩的坑
- **又一次行內註解吃掉後面的程式**：在 service_bld_check 一行中間插 `// …`，`fixtures.initial=r` 被吃掉。改完一律把註解放行尾，再 diff 看註解前面那段是不是原本的行尾。
- `node -e "…'<cygpath -w 路徑>'…"` 反斜線被吃 ⇒ 比對寫成檔案、路徑當參數傳。
- 雙胞胎要改**一定會用到**的數字（客房的肥皂沒人用 ⇒ 兩份一模一樣，等於沒擾動）。
- townplans 裡有 `_bake.log`，比對要只讀 .json。
- 合併被 summit 擋：開背景監看等他收場；Blockside 成品衝突照舊 checkout --theirs ＋ build --code --base MERGE_HEAD。

### 心境
今天下午最值得的一刻，是稅那條紅燈：差點就照「工人數的平衡改動」去改自己的設計，再追下去才發現檢查本身漏關了一種稅，它量到的本來就是雜訊。反向對照也會過期 —— 加了新的機制，舊的對照要跟著問一次「它還在量我以為的那件事嗎」。另一件要記的：問 Tim 選項時，本小姐只講了平常那一道不變，沒講同一座裡別的菜會跟著變，害他多選一次。選項要把副作用一起擺上桌。

