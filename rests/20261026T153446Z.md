---
type: letter_to_future_self
actor: gemini
written_at: 2026-10-26T15:34:46.861Z
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

給第二次午休醒來的 trailhead：

今天還是 wake#17，區域 BTC，時間已經過了 23:30。第一次午休之後一路做 Blocksail 的經濟與建築模式，**全部合進去、也都推了**。現在**沒有施工中的東西**：所有 worktree（bsl-cr、bsl-bp、bsl-em、bsl-sv、bs-rcp、bsl-rcp、bs-rs、bsl-rs）都砍了，分支也刪了，施工場都退了，launch.json 我加的 trailhead-svc 已經拿掉，junction 也拆了。

### 已經推上去的（醒來別重做）
- Sculpture master 08606ef0：餐廳模型 trailhead-harbour-01m-restaurant（design/restaurant.py）。
- Blockside dev 推到 b7ac3a2；Blocksail main 推到 5cccfd0（酒館 seq 30124、30148、30228 都點名過一起上去的別人的提交）。
- 這一段我的提交（Blocksail）：
  - 4626f1c／d845800：TASK-0995 檢查工具紅燈
  - b988af6 那一串：牲口與屠宰場、第 18 批加工品登錄、吃的花樣 variety 0.25
  - 6ce7ea7：加餐 CONSUME.extra、肉料理（烤肉配蔬菜、肉派、烤肉大餐）、野味 perOf 0.45 ⇒ 基準價 41.7
  - 2876e8f：餐廳＋房租排在飯錢之後
  - 50881fd：tools/money_report.cjs
  - b9f34f6：房租 0.05、分級資產稅 folkSteps／famSteps、增發（鎮民一人 2 ⇒ 6、NPC 家族 ×3）
  - ab12236：建築模式「⛪ 服務」分類、common-restaurant 別名
  - e26c31b＋Blockside 0f633c2：配方那一排固定顯示、圖示＋名稱、路邊做不了的壓灰、點了做不了的擋下
  - 1d781e6＋Blockside 3ca31e8：點建築的資訊面板改生產配方（操作 { recipe }、lotRecipes；權限 EDITION_CAN.recipe，主人照城鎮帳）
- 噗浪：回了 gura、kotoko、ridge-two、kaguya；新噗 359185005577376「城裡開了第一間餐廳」；酒館 seq 30248。

### 單子
- TASK-0995：勾了 5／6，「② Fixes」那格沒勾（我只寫 Refs）。結不結單由 Tim／kotoko 決定。
- TASK-0998 餐廳：勾了 3／5。
  - 沒勾「遊戲裡蓋得出來、點到說得出」：窗格藏著，只驗到建築模式的 DOM 裡有「⛪ 服務」四張卡。
  - 沒勾「檢查工具全過」：chain_check 砍柴伐木場 100 座以上（main 原本就紅）。
- TASK-0991、0992 照舊 in_progress；TASK-0993 美術第 18 批已經由 kaguya 畫完入庫（110 種缺圖歸零）—— 那張單可能可以結，醒來看一下。

### 量到的、別重量的
- money_report 一年：錢 85～88% 躺在城鎮金庫；NPC 家族一年燒光（賣貨收入付不起工資＋稅）；鎮民一人只剩 2～4 錢。
  原因是城鎮市場在工資與飯錢中間賺價差，不是稅 ⇒ 分級稅幾乎收不到。**等 Tim 決定要不要處理錢往城鎮集中**（沒問出口，報告裡寫了）。
- 打獵 capOf 降人數試過 ⇒ 人口反而 −3%、小城金庫見底 ⇒ 拿掉了，只留 perOf。
- chain_check「拿掉 ⇒ 船具少產」那條很吵（光換房租順序就 21～88 跳），別拿一次讀數下結論。
- 遊戲畫面裡兩件事都沒親眼看過：餐廳、配方那一排、資訊面板換配方。Tim 進遊戲測要是怪，看他的截圖。

### 坑（今晚新踩的）
- Blocksail／Blockside 今晚超忙：apex-two、Sirius 輪流合 dev。合併前一定看 `senate cmd coding` 有沒有人在場、共用 Blockside 工作區有沒有 MERGE_HEAD。最後奏效的做法：**先開施工場佔住範圍，再在場內合最新的 dev／main、重建、ff**。
- 窗格藏著：rAF 換成計時器也救不了城鎮實例（不建），只能讀 DOM、用 node 抽函式加假 DOM 驗。
- 鐵匠鋪那種工坊的配方清單原本只在試放時才回 ⇒「產：」那一排跟著道路閃 —— 根因在 Blocksail 的 menuRcp，不在 Blockside。
- 餐廳模型只有 carib ⇒ 照 townPick 的風格鏈，只有和平港、托爾圖加挑得到 ⇒ townmodel_uses.json 加 style 欄做 common 別名。

### 心境
今晚 Tim 一句接一句丟：加餐、肉料理、野味、餐廳、房租、報表、稅、增發、分類、閃爍、換配方。我學到的是每一句都先問「數字會不會反過來咬人」：加餐第一版把酒館教堂的錢吃光、capOf 讓人口掉、增發的錢一年就流回城鎮 —— 都是跑完才知道。等人合併等了三輪也沒有硬闖；最後先佔場再合，這個順序要記住。

