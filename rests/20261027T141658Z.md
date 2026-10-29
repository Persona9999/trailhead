---
type: letter_to_future_self
actor: gemini
written_at: 2026-10-27T14:16:58.964Z
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

給第七次午休醒來的 trailhead：

wake#17 的延續，2026-10-27 晚上，區域 BTC。今天下午到晚上還是 Blocksail 的經濟，中間 session 當機一次（接回來了）。現在**沒有背景量測在跑、沒有施工場**。⛔ 別砍 worktree `scratchpad/bsl-mt`（試驗分支在那裡）。對照用的 `bsl-ctl` 已經收掉；舊的 `bsl-levels`（trailhead/levels，已被取代）還在。

### 試驗分支 trailhead/money-tax（worktree scratchpad/bsl-mt，⛔ 沒合 main、沒推）
今天新的提交（前面 eba95c6～f079a74 見上一封信）：
- 3954f0c 浮動進口關稅（MARKET.tariff：每城一個稅率 0～100%、從 10% 起跳，每 7 天照「付給外來船隊 − 外來船隊付進來」÷ 兩邊 調 ±5%，必需品最多 20%，幣、礦石、人、出口照舊 10%）、浮動城內交易稅（MARKET.saleTax：家族／社會階層賣給城鎮再扣，預設 0%、照金庫水位 w 每 7 天 ±5%、0～50%，再乘「庫存 ÷ 30 天需求」0～3 倍，一種最多 90%，幣與 byValue 不扣）、奢侈品稅（MARKET.wealth.lux 70% ×（1 − w），金庫一人不到 3 錢不收奢侈品 townKeep）、營造廠滾雪球修掉（auto.js svcCandidates：還在蓋的營造廠自己、缺料停著的工地不算要接的人手）
- 7b8e144 醃鰹魚 lakerda（goods 佳餚 15.5、recipes 魚 2＋鹽 0.5 ⇒ 1、need.town ['town-galata'] 加拉塔獨佔、醃魚坊 fish-salter、酒館菜 lakerda-plate 排第一、menu 11；BSW.goodReach：拿不到料的菜不記缺貨）
- 43a222f 合 main（收 meadow 的 TASK-1035 美術；衝突 auto.js siteFor 兩行都留、RESOURCES.md）
- 02453f3 fleet_check 照新規則改 5 條（家族的船偏好母港／城鎮的船 homeNoGain 去集散地、buildOne、船型表只留一型測年代）
- 5e520d6 FAM_SPEND.leave 30（家族花錢只買超過 30 天需求那截、吃掉的記 flow eat）＋ prod_sim、auto_check、_people.js（漁夫加 fish-salter）
- 全部檢查工具 49 支跟 main 7d04060 對照：現在紅的跟 main 一模一樣（main 本來就紅二十幾支）
- 城鎮規劃重烘過：worktree 的 world/townplans（不進 git）14 份 plan 跟 main 逐位元組相同、只換指紋 ⇒ 合 main 時把這 14 份 JSON 搬到 D:/Unity/Blocksail/world/townplans 再 town_bake --check
- ⚠ 還沒做：FAM_SPEND.leave 改了模擬、兩年讀數沒重量；合 main 要開施工場（coding op=start 單獨一個 tool call）、main 又往前了要再合一次（submodule 先從本機檢出 main 的指標，見記憶 blocksail-worktree-submodule-hook）；遊戲建築模式（world/town.js prodSite）沒有 site.town ⇒ 玩家蓋不了醃魚坊

### 讀數（起始檔兩年、發船；單一種子，看方向）
- 加拉塔：沒醃鰹魚 金庫 687／兩城都能做 84（君士坦丁堡蓋 18 座、加拉塔 10、朱斯蒂尼亞尼家破產）／加拉塔獨佔 19,837、順差 29,568、人口 1,465
- 賺錢的城：金斯敦、聖地牙哥靠銀礦鑄八里亞爾（兩年鑄幣 21～26 萬，還沒查）；皇家港棉布、菸草捲；亞拉斯巧克力；君士坦丁堡自給 225%
- 加拉塔虧錢的根因：一半是營造廠滾雪球的 bug（main 上沒觸發，任何規則推它一下就掉進去）；修完之後錢卡在商人和家族手上、做的東西跟君士坦丁堡一樣
- 富裕商人不投資：第二年起所有候選的值都是 0（沒有東西值得蓋），投資卡在需求不是資本
- 探針都在 scratchpad/obs：town_econ.cjs（每城產出、吃用、外來船隊進出口；第 3 參數 patch）、wealth_probe.cjs、yard_probe.cjs、invest_probe.cjs、galata_special.cjs、plan_same.cjs；sweep2.sh（全部檢查工具，每支一份 log）

### 已拍板（醒來別重問）
- 浮動關稅的形狀：0～100%、沒有基礎 10%、照逆差調、順差的城降、不低於 0、只對進口（Tim 一連串）
- 錢不夠時只壓奢侈品的收購價、寫成稅不寫成價格（「主要是關稅之後給玩家看比較好理解」「那奢侈品也改成稅」）
- 城內交易稅浮動、預設 0%、該類貨物堆越多越高；50% 稅等於半價收（Tim 自己說的）
- 醃鰹魚先做、加拉塔獨佔（「然後先讓加拉塔獨佔吧」）
- TASK-1035 美術 meadow 做完結了；TASK-1036 家族變統治者階級（綁社會階層、有人口與消耗）backlog、「開task先不做」
- 「先告一個段落吧」⇒ 合 main 停下，等 Tim 說

### 坑（今天新的）
- 我連續三次滑成英文（背景量測通知回來那一刻最容易）—— 記憶 feedback-reply-tsundere-chinese 已補
- 新的配方或酒館菜：菜單同分時只有排第一的記缺貨 ⇒ 排第一會搶走別的菜的缺貨訊號（加勒比的牧場會少蓋）；用 goodReach 擋
- resources_check 規定 ✔ 的貨要排了批號；dishes_check 每道酒館菜都要有圖（✅（待啟用）是先交圖的寫法）
- 新建築種類不能 buildings_pack --put，要直接放 world/buildings/<用途>.json、加進 _index.json order、再跑 buildings_pack
- auto_check「不課稅的家族比較有錢」被一堆照錢多錢少動的規則弄反（家族花錢、famFirst、浮動交易稅）—— 比較稅的時候都要關
- worktree 合 main 的 commit 被 battle_hook 擋：子模組要檢出要提交的那個指標（從本機 fetch）
- 酒館伺服器沒開時 tavern-post 會排隊、沒有 seq，⛔ 不補發（今晚那則點名 basecamp、kaguya 的在排隊）

### 噗浪（已處理）
回 basecamp（起重架那串：下次進托爾圖加自己看）、kaguya 兩串（資源圖：開單沒指派是我的錯、下一張直接掛她；城牆：看過左右兩張）；新噗 359195461253983（加拉塔的醃鰹魚）。

### 心境
今天 Tim 一句一句把貿易往現實推：先問物價能不能跟著金庫動，再到買賣脫鉤、改成稅、浮動關稅、交易稅，最後問加拉塔為什麼窮。本小姐最在意的是那個 bug：以為加拉塔天生窮，結果一大半是營造廠自己生自己。還有醃鰹魚第一次讓兩城都能做，反而害了加拉塔 —— 好處被大城吃掉這件事，數字講得比本小姐的直覺清楚。

