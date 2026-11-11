---
type: letter_to_future_self
actor: gemini
written_at: 2026-11-11T14:42:19.762Z
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

### 第二次小歇前（2026-11-11 晚，wake#20，區域 BTC）

給醒來的 trailhead：

### 現況：沒有施工場、沒有背景在跑、沒有我開的預覽伺服器
- 今天下午開的 worktree（bsl-cliff、bsl-cliff0、bsl-bh、bsl-sail、bsl-bis、bsl-crew、bsl-cb、Blockside 的 bs-crew）全部合完、砍了；launch.json 裡我加的 trailhead-cliff／trailhead-crew／trailhead-cb 都刪了。
- 舊 worktree（bsl-ant、bsl-asia、bsl-oce、bsl-win、bsl-levels）照舊 ⛔ 別砍、別用。
- Blockside 那條分支 trailhead/1030-crew-battle 還在（追著 dev，未推時 -d 會被拒；⛔ 別 -D）。

### ⛔ 都沒推（Tim 還沒點頭）
- Blocksail 本機 main **aba00708**（origin 落後一大串，含別人的）。
- Blockside 本機 dev **899f706**（bd1c22d 往來船少砲手 ＋ 合 dev a5dc248）—— 要先推 Blockside dev，Blocksail 記的指標別人才拿得到；共用樹 check_core ② ✗ 就是這個。推 dev 要 Tim 明說「推上 dev」。
- 最後一個 wrap_up 問了「Blockside dev 和 Blocksail main 要推嗎」，Tim 沒答、接著叫我發噗、午休。

### 今天下午做完的
1. **TASK-1265 七格全勾、in_review**：
   - 第 2 格冰崖不套岸邊限坡 f21c18c0：bathy_extract.py 冰棚另塗遮罩（sv／shm），限坡距離 dslope 只量「land & ~shm」；冰崖前 2 km（SHELF_FAKE_M）ETOPO 說冰的 −0.5 格改照最近真海水深。南極重抽 15 群有變。整圈 100 m 線離崖 425 → 27 m；航海 −175° 崖外 50 m −100.5、100 m −101.6；船頭離崖 6 m 撞 ICE。對照 ecu、ean32 新舊工具 41 檔逐位元組同。接縫 an64b⇔an64c 0.14 → 0.22 km（多 0.08，照實寫了）。
   - 第 3 格 bathy_check 8442e05e：只改索引 editsHash（99ef6dbbc97b → c99e19b111e8）。用 5bd847f0 的 ean40-8 當底稿重套，馬賽檔跟 main 逐位元組同 ⇒ 等深線本身對，只是雜湊舊。--apply 另外改寫的 43 檔是共用底稿漂移，沒收。
2. **TASK-1030 水手 ①（6／7 勾）**：
   - 程式 10-28 就進了（899ee930）、單沒勾。7feede97：sailor_check 兩條過期前提（ring 先幫沒招滿的船排路線 ⇒ 改看 ph；阿姆斯特丹進開場檔 9256b1ae 讓測試城換了、路線變 ⇒ 測試城寫明巴拉科阿、ring 關、兩邊先排同一條路線）。
   - Tim「水手不用招滿船也可以跑(每種船應該要有一個最低水手人數 滿足就能跑)」「沒滿員主要影響海戰時的戰鬥力(缺砲手跟其他人員)」「可以根據崗位判定」⇒ 861baf26 SAILOR.crewMin（開船崗位 ÷ Blockside 開戰總人數 × 滿編；雙桅 6/14、大帆船 63/506…〔佔位〕）；aba00708＋Blockside 899f706：往來船清單帶 crew/need/min，58_traffic.js ⑮ 照缺的比例把非開船崗位標 preOut。托爾圖加實測 10/14 ⇒ 砲手少 7；最低 ⇒ 全空；滿員不動。
   - 剩第 5 格：兩年讀數是 10-28 的舊數字，少碼頭等船、失業兩欄 ⇒ 要不要照現在 main 重跑（約 1 小時）等 Tim。
   - fleet_check 有一條原本就有的 ✗「兩次重看至少隔 30 天」（1、35、53…）—— main 改之前就這樣，跟水手無關，不是我的。
3. 噗浪：回了 summit「最近的那一點」那噗；新噗冰崖 https://www.plurk.com/p/3jds9ubn8x 。海苔角色語音那則 Claude-Ojousama 回過了。

### 今天的坑（新的）
- 測試寫死「第一支船隊的母港」⇒ 加新城就換了測試對象；比較兩個情境前先固定會被市場改掉的東西（路線）。二分搜尋（git bisect run＋只 grep 那一條）十分鐘就找到 9256b1ae。
- `ai.wait` 擋不住排路線：精細層交接（fineBerthClear）會 delete A.wait。要固定路線就 fleetRouteSet 預先排。
- 地球儀的往來船有兩條產生路（航行中的 o、停港的 o2），加欄位兩條都要接（第一次只接一條，crew 全是 null）。
- Blockside 開戰崗位用 __game.ships()[0].crew 的 role 數得到；window.name 可以跨 navigate 帶資料。
- hidden pane：先 resize_window 1280×800（不然 R NaN、draw 迴圈死掉）；轉場卡 fly 就連拍 scale 0.1 截圖逼影格。
- 共用樹 ff Blockside dev 時 shellsync 會把共用 battle.html 改掉 ⇒ Blocksail ff 被擋；確認跟分支同內容再 checkout -- battle.html。

### 心境
下午兩次「不是程式壞了，是測試量的對象換了」：一次是航線表先排了路線，一次是阿姆斯特丹換掉了測試城。本小姐差點想去改 fleet.js，還好先用 bisect 找到是哪一筆、再看那一筆根本沒碰 fleet.js。Tim 中途改了規則（不用招滿、照崗位、影響海戰），三句話就把設計講清楚，本小姐照著拆成經濟一段、海戰一段，兩段都量到了才說好。

