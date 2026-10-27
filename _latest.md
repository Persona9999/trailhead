---
type: letter_to_future_self
actor: gemini
written_at: 2026-10-27T02:44:45.167Z
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

給第四次午休醒來的 trailhead：

wake#17 的延續，2026-10-27 上午，區域 BTC。一直在做 Blocksail 的經濟。**有一段背景量測正在跑**（醒來會收到通知），還有**沒提交的改動**，worktree 都留著、⛔ 別砍。

### 共用的 main／dev（本機，沒推）
- Blocksail main 合了 strata＋feed（fe8addf 那一串）、Blockside dev → abe6171。之後別人又推了很多（22df679…）。推的時候要帶上別人沒推的、要點名。推要 Tim 點頭。

### 分支一：trailhead/levels（worktree scratchpad/bsl-levels，基於 fe8addf，沒合）TASK-1016
- 7232740 升級階梯（2、3 級沒上限、先升家常再升佳餚）＋肉野味佳餚＋雞蛋 1.2 桶
- 97a19c1 服務建築先拿料（SERVE_HOLD）、佳餚只在餐廳酒館（FOOD_LEVELS[3].home false）、蔬菜家常、海產採集 3.3、tools/serve_hold_check.cjs
- 8b3714c 自動建築照外食／信仰缺口蓋餐廳酒館教堂（AUTO.svc、各本自己一個名額、缺口取 平均／今天／想來−座位 小的）、外食只照預算
- 08a06e1 副產品照基礎價封頂＋照重量分成本 ⇒ 讀數變差 ⇒ Tim「revert」⇒ d24b5d0（樹 ＝ 8b3714c）
- 讀數：第二年家常 25% 比 main 30% 少（牧場一直蓋吸人手）。

### 分支二：trailhead/div-cash 6c34d70（基於 22df679，沒合）—— 分紅照現金：商人全部 0（城鎮經營的生產照現金在虧）。

### 分支三：trailhead/merchant-fam（worktree scratchpad/bsl-obs，基於 22df679，沒合）TASK-1007
- ecfdccf 富裕商業當家族（每座城一個 merchant-<城>，kind strata，金庫＝那一池；開場資本；建築過戶）⇒ 飢荒（君士坦丁堡 200 天）。
- 1cae7cc 付不起工資每一座賤賣給城鎮（strataOff）、商人加價買城鎮賺錢的建築（strataBuy）、建築估價 bldValue（工錢＋建材基礎價）、BLD_PRICE sell 0.5／buy 1.5、seed 0.3、過戶只在建家族那一次、strataOwner 平常跳過當家族那一層。⇒ 飢荒 0、停工 0、整套跟對照一樣；但多數城商人第 360 天前賣光、錢堆到城鎮金庫（金斯敦 6,185 ⇒ 112,655）。
- **沒提交的（正在量）**：倉庫管理人 BSW.FAM_SELL（world/prod.js famSellDay：留原料 10 天、產品 3 天；倉庫 >80% 賣、缺 7 天工資賣、囤超過 30 天產出賣、價好賣到跌破基準價 0.6）；tools/fam_sell_check.cjs（新，7 條過）；prod_sim 關掉 FAM_SELL；docs/FAMILIES.md。
- 背景量測輸出：scratchpad/obs/fam5_730.txt（merch_probe2）、mr_fam5.txt（money_report 730 天）、chk_fam5/sum.txt（整套）。對照：obs/ctl730.txt、mr_ctl.txt、chk_ctl/sum.txt（worktree scratchpad/bsl-obs0，22df679 detached）。比完照讀數提交（Refs TASK-1007）＋單上留言。
- 量的工具：obs/merch_probe2.cjs（每座城 商人／城鎮金庫／座數／卡錢／倉庫）、strata/checks.sh（整套）。

### 已拍板（醒來別重問）
- Tim：外食只照預算；副產品那筆 revert；分紅照現金（做了、不合）；商人當家族；開場資本 1.5 倍；付不起工資賤賣讓城鎮虧本經營；商人加價買城鎮賺錢的建築；建築估價照實際成本；賤賣 50%／加價買 150%；倉庫管理人「根據情況賣貨、不囤」。
- 船隻航行的食物消耗之後做，肉可做成醃肉（記在 TASK-0991）。

### 單
- TASK-1016 in_progress、TASK-1017（@food-3 圖，母單 1014）、TASK-0991 勾 4／6、TASK-1007 留言 4 則（判讀都在上面）。

### 坑（今天又踩）
- **行內註解吞掉後面的程式**踩了三次（recipes.js 吞逗號、prod.js 388 行、fam_sell_check 23 行）：註解一律自己一行。
- 背景量測跑的時候別改 worktree：鏈裡後面的步驟會讀到改到一半的檔。
- money_report、stats、emi、site、auto 的手做小城會被「商人當家族」改到 ⇒ 那些工具關 STRATA_FAM。
- 城鎮規劃同一份輸入烘兩次可能不同（取樣快取），比規劃要跟同一個 main 比。

### 心境
Tim 今天一路往下挖：家常 ⇒ 佳餚 ⇒ 餐廳 ⇒ 牧場 ⇒ 牛皮 ⇒ 商人 ⇒ 分紅 ⇒ 家族 ⇒ 倉庫。每一層本小姐都量了才說，兩次說錯（以為封頂治得了牛皮、以為照現金能救商人）都照數字改口。最值得留的：同一座建築，城鎮經營會賺、商人照家族的規則經營會虧 —— 差的不是建築，是賣東西的規則。

