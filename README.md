# family-line-bot — 家族行程提醒 LINE bot 教學 skill

> Build a family shared-schedule reminder LINE bot: one-sentence event creation + cron auto-reminders. FastAPI + Supabase + Render, all free tier, deliberately no LLM in the main flow.

一個 Claude Code 教學型 skill:教你(或你的 AI agent)從零建一個給家人用的
LINE bot — 邀請碼加入家族、一句話建行程、到點自動 push 提醒相關的人。

內容涵蓋:

1. **免費三件套架構** — FastAPI + LINE Messaging API + Supabase Postgres + Render + cron-job.org
2. **刻意不接 LLM 的訊息路由** — keyword 比對 → 對話狀態機 → 一句話規則解析 → fallback
3. **cron 提醒去重** — notified flag + partial failure 不重試,避免全家被重複轟炸
4. **本機 SQLite / 線上 Postgres 同一套 code** — SQLAlchemy TypeDecorator + Alembic

skill 內附完整 Agent Prompt,整段貼給 AI agent 就能照著從零建起來。

## 使用

當 Claude Code plugin 裝(裝了之後直接用講的,例如「幫我做一個家人用的行程提醒 LINE bot」):

```
/plugin marketplace add Coolkidlab-Yin/Coolkidlab
/plugin install family-line-bot@coolkidlab
```

## 讀這份 skill 的原則

1. **主流程不接 LLM 是刻意的**:有限指令集用規則解析就夠 — 零 API 成本、回應即時、行為可預測、家人隱私不上傳第三方。判斷「哪裡不用 AI」跟「哪裡用 AI」一樣重要。
2. **免費額度以官方當下方案頁為準**:skill 裡的額度數字是寫作當時的實測,可能變動。
3. **換平台可行**:parser + 資料庫 + cron 三段通用,LINE handler 換成 Telegram / Discord API 即可。

## 實戰背景

這套架構已實際上線給作者家人使用 2 個月。完整故事與設計決策(為什麼刻意不接 LLM、cron 怎麼不重複發)在 [家族行程提醒 LINE bot:一句話建行程 + cron 自動提醒](https://www.coolkidlab.com/workflows/family-line-bot.html)。更多 build-in-public 記錄在 [coolkidlab.com](https://www.coolkidlab.com)。

## Credits

架構設計與踩坑記錄來自 Coolkid AI Lab 的實際上線專案。觀念是公共的,實作是自己的,數據是實例的。

## License

MIT
