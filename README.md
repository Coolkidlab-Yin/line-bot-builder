# line-bot-builder — LINE bot 建造器

> Build a LINE bot for anything — customer support, booking, reminders, coaching check-ins, internal tools, notifications. Covers credential setup, webhook signature verification, push dedup, and the one decision that matters most: whether your bot needs an LLM at all.

這是一個 Claude Code 教學型 skill:裝了之後直接跟 Claude 說你想做什麼樣的
LINE bot,它會照 SOP 一步步帶你建起來 —— **包含開瀏覽器陪你把 channel
token 申請下來**,那是多數人卡住的地方。

## 用途你自己決定

這份骨架不預設你要做什麼。skill 的第一步就是問你要做哪一種:

| 型態 | bot 做什麼 | 要不要 LLM |
|---|---|---|
| 客服 / 常見問答 | 回答問題,答不出來轉真人 | 要,但要接你自己的資料 |
| 預約 / 報名 | 引導填時段、留資料、確認 | 不用,狀態機就夠 |
| 行程 / 待辦提醒 | 建行程、到點提醒 | 不用 |
| 個人教練 / 打卡 | 收紀錄、給回饋、追進度 | 看回饋要多客製 |
| 通知推播 | 幾乎不收訊息,只負責發 | 不用 |
| 內部小工具 | 查東西、觸發流程 | 通常不用 |
| **以上都不是** | **你講,照你的做** | — |

選哪一種決定要建哪幾個元件 —— **通知推播型只要三個,客服型只要三個**,
不用照抄全套。

## 最重要的一個判斷:要不要接 LLM

skill 會先帶你做這個判斷,因為**接錯邊會浪費最多時間**:

- **指令集固定**(建行程、報名、打卡、查詢)→ keyword 比對 + 規則解析就夠。
  零 API 成本、回應即時、行為可預測、使用者輸入不出你的伺服器。
- **使用者會亂問**(客服、問答)→ 才需要 LLM,而且要先想清楚哪些內容
  會被送到第三方。
- **多數情況的正解是折衷**:主流程走規則,LLM 只當 fallback。

判斷「哪裡不用 AI」跟「哪裡用 AI」一樣重要。

## 安裝

```
/plugin marketplace add Coolkidlab-Yin/Coolkidlab
/plugin install line-bot-builder@coolkidlab
```

裝完直接說「我想做一個 LINE bot」,Claude 會先問你要做哪一種。

> **這是教學型 skill,不是現成的 bot。** repo 裡沒有可以直接跑的程式碼 ——
> 它教 Claude Code 帶著你從零寫出來,包含怎麼申請憑證。

## 開工前你要先有的東西

**channel token 不用自己研究怎麼申請。** skill 的第 0.5 步會讓 AI 開瀏覽器
陪你走到拿到為止,你只負責打帳密、按送出。

| 要準備的 | 說明 |
|---|---|
| LINE Developers 帳號 | 建 Provider 與 Messaging API channel,免費 |
| 一個公開的 HTTPS 網址 | webhook 必須是公開 HTTPS;本機開發用 ngrok 之類拿臨時網址 |
| 資料庫 | 本機先 SQLite 跑通、上線再換雲端 Postgres 是低風險路線 |

**先算一次額度**:LINE 的**主動推播訊息有月上限**,免費方案不多。
主功能是推播的型態(提醒、通知、教練打卡)最容易撞到 ——
使用者數 × 每天推播次數,先算再決定架構。

## 已含的實戰坑(原作者實測)

- **官方自動回覆沒關掉**:bot 回什麼都被罐頭訊息蓋掉,而且看不出問題在哪
- **cron 每分鐘跑,同一筆提醒重複發**:發完必須立刻標記,不然下一分鐘又撈到
- **部分失敗不要重試整筆**:重試會害已經收到的人再收一次
- **派送端點裸奔**:cron 打的端點一定要驗 secret header
- **webhook 不驗簽**:等於任何人都能冒充使用者對你的 bot 說話
- **使用者不會用 bot**:不解釋,直接示範一次讓他看

## 原作者實跑過的版本

skill 裡附了一份完整的範例 recipe:作者自己家裡在用的**家族行程提醒**bot
—— 一句話建行程、cron 到點 push 提醒相關的人、刻意不接 LLM。上面所有的坑
都是從那個版本踩出來的。拿它當填法參考,不是你一定要做的東西。

完整故事與設計決策在
[家族行程提醒 LINE bot](https://www.coolkidlab.com/workflows/family-line-bot.html)。
更多 build-in-public 記錄在 [coolkidlab.com](https://www.coolkidlab.com)。

## 換平台可行

路由 + 資料庫 + 推播三段通用,LINE handler 換成 Telegram / Discord / Slack
的 API 即可。台灣使用者最常用 LINE,所以原版做 LINE。

## License

MIT
