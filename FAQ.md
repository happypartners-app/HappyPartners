# Frequently Asked Questions

**Language:** [English](#english) · [繁體中文](#zh-tw) · [简体中文](#zh-cn)

---

<h2 id="english">English</h2>

Quick answers to common questions. If yours isn't here, [open an issue](https://github.com/happypartners-app/HappyPartners/issues) or email [hello@happypartners.app](mailto:hello@happypartners.app).

### General

#### How is HappyPartners different from ChatHub / other multi-AI tools?

A few practical differences:

- **Native macOS app**, not a browser extension. Better integration, better performance, no Chrome-only dependency.
- **16 AI platforms including 8 Chinese ones** (DeepSeek, Kimi, Qwen, Yuanbao, Doubao, MiMo, Z.ai, ChatGLM). Most Western tools skip these entirely.
- **Cross-AI relay via Alt+drag** — drag any AI's reply onto another AI to hand off context. Or broadcast to everyone.
- **Full local history with Chinese-friendly full-text search** (FTS5 trigram). Your conversations stay on your Mac, searchable across months.
- **Projects** — group related conversations, each project has its own AI lineup and layout.
- **Uses your existing AI subscriptions**, not API keys. No token metering, no extra bills.

#### Why macOS only? What about Windows / Linux?

Priority. HappyPartners is built by one person. Shipping to one platform well beats shipping to three platforms badly.

macOS was chosen because:
- The target user (engineers, designers, PMs, researchers doing multi-AI workflows) is over-indexed on Mac
- Electron + WebKit + `webview` behaviour is better-characterized on macOS
- Apple Silicon performance makes running 4 browsers-in-an-app tolerable

Windows support is being evaluated based on real Beta demand signals — not ruled out, but not current focus.

#### Why not just use the official APIs?

Because using APIs would mean:
- You'd pay for token usage on top of your existing subscriptions
- You'd need to manage API keys for every platform
- The behavior would diverge from the official web interface (different features, different limits)
- Most AI platforms' terms of service discourage wrapping their API for a different product

HappyPartners is designed so **you use your own AI subscriptions exactly as you do today** — we just unify the way you operate them. No token cost, no middle layer.

### Beta program

#### When does Beta end?

No fixed date. Beta is gated by product readiness, not calendar. Rough signals that will end Beta:
- Stability: no major bugs, all 16 platforms reliably working under real daily use
- Workflow completeness: key user patterns covered
- Pricing decision: post-Beta pricing confirmed based on real Beta usage data

Expect Beta to run for several more months.

#### Will it cost money after Beta?

Yes. HappyPartners is a commercial product.

Current plan (tentative, subject to Beta feedback):
- **Monthly**: ~$12 USD
- **Yearly**: ~$99 USD
- **Early Bird discount** for existing Beta users

Pricing is intentionally lower than competing tools like ChatHub ($15–$25/mo).

Beta users will get first notice and an early-bird price before general release.

#### My Beta license expired. What now?

Beta keys are 30-day trials that start when you first activate. When it expires, the app returns to the license screen. During active Beta, contact [hello@happypartners.app](mailto:hello@happypartners.app) for an extension — it's usually a yes.

Post-Beta, expired trials convert to a paid subscription or stop working (your data remains on your Mac either way).

### Privacy & data

#### Can you see my conversations?

**No.**

- Your conversations with AIs happen directly between your Mac and each AI platform — HappyPartners is not in the middle.
- Conversation text is stored locally in a SQLite database at `~/Library/Application Support/HappyPartners/`.
- We don't upload conversation content anywhere. Ever.

What we do collect (optional, opt-out in Settings):
- Anonymous usage events (feature triggered, errors occurred) — random UUID, no personally identifiable info
- Event *type* only, never event *content*

#### What about my AI platform logins?

Each platform's login stays inside its own Electron `webview` partition on your Mac. HappyPartners never reads your cookies, tokens, or sessions programmatically. If you log out of an AI in HappyPartners, the session is removed from your Mac.

#### Is the source code open?

No. HappyPartners is a commercial closed-source product. This public repository hosts release notes, documentation, and issue tracking only.

#### Where is the backend hosted?

Cloudflare:
- **Workers** for license verification, waitlist signup, anonymous analytics
- **R2** for the app DMG and update files
- **Pages** for the website

No third-party analytics (Google Analytics, Mixpanel, etc.). No third-party error tracking (Sentry, etc.).

### Technical

#### Does it auto-update?

Yes. Every launch, the app checks for a newer version. If one's available, a small banner appears at the top. You click "restart to update" when convenient. Updates are downloaded differentially (you don't re-download the whole app each time).

#### Does it work offline?

No. AI platforms require a live connection. HappyPartners itself works offline to read your history, but you can't send new prompts without internet.

#### Why is it 156 MB? That feels big for a "wrapper".

It's a native Electron app including Chromium + Node + the runtime for 17 simultaneous webviews. It's comparable in size to Slack, Discord, VS Code. On Apple Silicon with ample RAM this is negligible.

#### Can I use one license key on multiple Macs?

Currently, one key is bound to one `machineId` on first activation. If you need to move to a new Mac, email [hello@happypartners.app](mailto:hello@happypartners.app) — we can transfer.

Multi-device support may come post-Beta depending on demand.

### Meta

#### Who builds HappyPartners?

One person, working independently. That's why response times aren't always instant and why the roadmap is driven by the most common use cases rather than every request.

#### Can I contribute code?

No — the source is private. You can contribute by:
- Reporting bugs with reproducible steps
- Suggesting features tied to real workflows
- Using the app during Beta and giving honest feedback

All three are genuinely valuable.

#### Will there be a Discord / Telegram community?

Maybe, eventually. Right now it would be a maintenance burden for little benefit. GitHub Issues + email is enough to cover Beta. If and when the user base grows to justify a community space, one will be opened.

---

<h2 id="zh-tw">繁體中文</h2>

常見問題快速回答。如果下面沒有你的問題，歡迎[開 issue](https://github.com/happypartners-app/HappyPartners/issues) 或寄 email 到 [hello@happypartners.app](mailto:hello@happypartners.app)。

### 一般問題

#### HappyPartners 跟 ChatHub 或其他多 AI 工具有什麼不一樣？

幾個實際差異：

- **macOS 原生 app**，不是瀏覽器擴充。整合更好、效能更佳、不綁 Chrome。
- **16 個 AI 平台，包含 8 個中文平台**（DeepSeek、Kimi、Qwen、Yuanbao、Doubao、MiMo、Z.ai、ChatGLM）。西方工具通常完全不支援這些。
- **Alt + 拖曳跨 AI 接力** — 把任一 AI 的回覆拖到另一個 AI 繼續處理。或一次廣播給所有其他 AI。
- **完整本機歷史 + 中文友善全文搜尋**（FTS5 trigram）。所有對話存在你的 Mac 上，可跨月搜尋。
- **專案（Projects）** — 相關對話歸組，每個專案有自己的 AI 組合與版面。
- **使用你原本的 AI 訂閱帳號**，不需 API Key。沒有 token 計費、沒有額外帳單。

#### 為什麼只做 macOS？Windows / Linux 呢？

優先順序問題。HappyPartners 由一個人獨立開發，**把一個平台做到好，勝過三個平台都做得普通**。

選 macOS 的原因：
- 目標使用者（工程師、設計師、產品經理、研究者）Mac 使用率偏高
- Electron + WebKit + `webview` 在 macOS 上行為較穩定
- Apple Silicon 效能足以同時跑 4 個內嵌瀏覽器

Windows 支援會依 Beta 真實需求訊號來評估 — 不是永遠不做，只是目前不是重點。

#### 為什麼不直接用官方 API？

因為走 API 會：
- 你要在原本訂閱費之外**多付 token 費**
- 要管理每個平台的 API Key
- 行為會跟官方網頁版**不一致**（功能、限制都不同）
- 多數 AI 平台的服務條款**不鼓勵**以 API 包裝成另一個產品

HappyPartners 的設計是讓**你用自己原本的 AI 訂閱帳號**，只是把操作方式統合起來。沒有 token 費、沒有中間層。

### Beta 計畫

#### Beta 什麼時候結束？

沒有固定日期。Beta 結束的標準是**產品準備度**，不是日曆。大致指標：
- 穩定度：沒有重大 bug，16 個平台在真實使用中都可靠
- 工作流完整性：核心使用模式都涵蓋到
- 定價決定：根據 Beta 真實使用數據確認正式版定價

Beta 預計還會跑幾個月。

#### 正式版會收錢嗎？

會。HappyPartners 是商業產品。

目前定價規劃（暫定，Beta 結束後依反饋調整）：
- **月費**：約 $12 USD
- **年費**：約 $99 USD
- **早鳥折扣**給現有 Beta 使用者

定價刻意**低於**競品如 ChatHub（$15–$25/月）。

Beta 使用者會**先收到通知**和早鳥價，才會公開開賣。

#### 我的 Beta license 過期了怎麼辦？

Beta key 是 30 天試用，從你第一次啟用起算。過期後 app 會回到 license 輸入視窗。Beta 期間聯絡 [hello@happypartners.app](mailto:hello@happypartners.app) 延期 — 通常都會 OK。

Beta 結束後，過期試用會轉成付費訂閱或停止使用（你的資料會繼續存在 Mac 上）。

### 隱私與資料

#### 你們看得到我的對話嗎？

**看不到。**

- 你和 AI 的對話是**你的 Mac 和各個 AI 平台之間**直接進行，HappyPartners 不在中間
- 對話文字存在本機 SQLite 資料庫：`~/Library/Application Support/HappyPartners/`
- 我們**絕不會**把對話內容上傳到任何地方

我們會收集的（可選，設定可關閉）：
- 匿名使用事件（觸發哪個功能、發生錯誤等）— 隨機 UUID，沒有個資
- 只記錄**事件類型**，絕對不記錄**事件內容**

#### 那我的 AI 平台登入呢？

每個平台的登入狀態只存在它自己的 Electron `webview` partition 中，在你 Mac 本地。HappyPartners **從不**用程式讀你的 cookies、tokens、或 sessions。若你在 HappyPartners 中登出某個 AI，session 會從你的 Mac 上清除。

#### 原始碼開源嗎？

不開源。HappyPartners 是閉源商業產品。這個公開 repo 只用於 release notes、文件、問題追蹤。

#### 後端放在哪？

Cloudflare：
- **Workers** — License 驗證、Waitlist 報名、匿名分析
- **R2** — App DMG 和更新檔
- **Pages** — 官網

**沒有第三方分析**（Google Analytics、Mixpanel 等）。**沒有第三方錯誤追蹤**（Sentry 等）。

### 技術面

#### 會自動更新嗎？

會。每次開 app 時會檢查新版。有新版時畫面上方會跳出小 banner，你點「重啟更新」即可。更新走**差分下載**（不是每次都下載整個 app）。

#### 可以離線使用嗎？

不行。AI 平台需要網路連線。HappyPartners 本身離線可以讀歷史紀錄，但不能送新訊息。

#### 為什麼 app 有 156 MB？這對「wrapper」來說很大。

它是完整的 Electron app，包含 Chromium + Node + 同時跑 17 個 webviews 的 runtime。大小跟 Slack、Discord、VS Code 差不多。在 Apple Silicon 記憶體充足的 Mac 上可以忽略。

#### 一把 license key 可以用在多台 Mac 嗎？

目前一把 key 在第一次啟用時綁定一台 `machineId`。若你換 Mac 需要搬移，email [hello@happypartners.app](mailto:hello@happypartners.app) 我們協助轉移。

多裝置支援可能會在 Beta 後依需求推出。

### 其他

#### 誰做的？

一個人，獨立開發維護。這也是為什麼回覆不一定即時、roadmap 以最常見的使用情境為主而不是每個單一需求。

#### 可以貢獻 code 嗎？

不行 — 原始碼為私有。你可以用以下方式貢獻：
- 回報可重現的 bug
- 提出跟真實工作流相關的功能建議
- Beta 期間使用並給誠實的反饋

這三種都是真實的價值。

#### 以後會開 Discord / Telegram 社群嗎？

也許，某一天會。目前開了只會增加維護負擔、效益不大。Beta 階段 GitHub Issues + email 就夠用。等使用者規模成長到值得社群空間時，會開。

---

<h2 id="zh-cn">简体中文</h2>

常见问题快速回答。如果下面没有你的问题，欢迎[开 issue](https://github.com/happypartners-app/HappyPartners/issues) 或寄 email 到 [hello@happypartners.app](mailto:hello@happypartners.app)。

### 一般问题

#### HappyPartners 跟 ChatHub 或其他多 AI 工具有什么不一样？

几个实际差异:

- **macOS 原生 app**，不是浏览器扩展。整合更好、效能更佳、不绑 Chrome。
- **16 个 AI 平台，包含 8 个中文平台**（DeepSeek、Kimi、Qwen、Yuanbao、Doubao、MiMo、Z.ai、ChatGLM）。西方工具通常完全不支持这些。
- **Alt + 拖拽跨 AI 接力** — 把任一 AI 的回复拖到另一个 AI 继续处理。或一次广播给所有其他 AI。
- **完整本机历史 + 中文友善全文搜索**（FTS5 trigram）。所有对话存在你的 Mac 上，可跨月搜索。
- **项目（Projects）** — 相关对话归组，每个项目有自己的 AI 组合与版面。
- **使用你原本的 AI 订阅账号**，不需 API Key。没有 token 计费、没有额外账单。

#### 为什么只做 macOS？Windows / Linux 呢？

优先顺序问题。HappyPartners 由一个人独立开发，**把一个平台做到好，胜过三个平台都做得普通**。

选 macOS 的原因:
- 目标使用者（工程师、设计师、产品经理、研究者）Mac 使用率偏高
- Electron + WebKit + `webview` 在 macOS 上行为较稳定
- Apple Silicon 效能足以同时跑 4 个内嵌浏览器

Windows 支持会依 Beta 真实需求讯号来评估 — 不是永远不做，只是目前不是重点。

#### 为什么不直接用官方 API？

因为走 API 会:
- 你要在原本订阅费之外**多付 token 费**
- 要管理每个平台的 API Key
- 行为会跟官方网页版**不一致**（功能、限制都不同）
- 多数 AI 平台的服务条款**不鼓励**以 API 包装成另一个产品

HappyPartners 的设计是让**你用自己原本的 AI 订阅账号**，只是把操作方式统合起来。没有 token 费、没有中间层。

### Beta 计划

#### Beta 什么时候结束？

没有固定日期。Beta 结束的标准是**产品准备度**，不是日历。大致指标:
- 稳定度：没有重大 bug，16 个平台在真实使用中都可靠
- 工作流完整性：核心使用模式都涵盖到
- 定价决定：根据 Beta 真实使用数据确认正式版定价

Beta 预计还会跑几个月。

#### 正式版会收钱吗？

会。HappyPartners 是商业产品。

目前定价规划（暂定，Beta 结束后依反馈调整）:
- **月费**：约 $12 USD
- **年费**：约 $99 USD
- **早鸟折扣**给现有 Beta 使用者

定价刻意**低于**竞品如 ChatHub（$15–$25/月）。

Beta 使用者会**先收到通知**和早鸟价，才会公开开卖。

#### 我的 Beta license 过期了怎么办？

Beta key 是 30 天试用，从你第一次启用起算。过期后 app 会回到 license 输入窗口。Beta 期间联络 [hello@happypartners.app](mailto:hello@happypartners.app) 延期 — 通常都会 OK。

Beta 结束后，过期试用会转成付费订阅或停止使用（你的数据会继续存在 Mac 上）。

### 隐私与数据

#### 你们看得到我的对话吗？

**看不到。**

- 你和 AI 的对话是**你的 Mac 和各个 AI 平台之间**直接进行，HappyPartners 不在中间
- 对话文字存在本机 SQLite 数据库：`~/Library/Application Support/HappyPartners/`
- 我们**绝不会**把对话内容上传到任何地方

我们会收集的（可选，设置可关闭）:
- 匿名使用事件（触发哪个功能、发生错误等）— 随机 UUID，没有个人信息
- 只记录**事件类型**，绝对不记录**事件内容**

#### 那我的 AI 平台登录呢？

每个平台的登录状态只存在它自己的 Electron `webview` partition 中，在你 Mac 本地。HappyPartners **从不**用程序读你的 cookies、tokens、或 sessions。若你在 HappyPartners 中登出某个 AI，session 会从你的 Mac 上清除。

#### 源代码开源吗？

不开源。HappyPartners 是闭源商业产品。这个公开 repo 只用于 release notes、文档、问题追踪。

#### 后端放在哪？

Cloudflare:
- **Workers** — License 验证、Waitlist 报名、匿名分析
- **R2** — App DMG 和更新文件
- **Pages** — 官网

**没有第三方分析**（Google Analytics、Mixpanel 等）。**没有第三方错误追踪**（Sentry 等）。

### 技术面

#### 会自动更新吗？

会。每次开 app 时会检查新版。有新版时画面上方会跳出小 banner，你点「重启更新」即可。更新走**差分下载**（不是每次都下载整个 app）。

#### 可以离线使用吗？

不行。AI 平台需要网路连线。HappyPartners 本身离线可以读历史纪录，但不能发新消息。

#### 为什么 app 有 156 MB？这对「wrapper」来说很大。

它是完整的 Electron app，包含 Chromium + Node + 同时跑 17 个 webviews 的 runtime。大小跟 Slack、Discord、VS Code 差不多。在 Apple Silicon 记忆体充足的 Mac 上可以忽略。

#### 一把 license key 可以用在多台 Mac 吗？

目前一把 key 在第一次启用时绑定一台 `machineId`。若你换 Mac 需要搬移，email [hello@happypartners.app](mailto:hello@happypartners.app) 我们协助转移。

多设备支持可能会在 Beta 后依需求推出。

### 其他

#### 谁做的？

一个人，独立开发维护。这也是为什么回覆不一定即时、roadmap 以最常见的使用情境为主而不是每个单一需求。

#### 可以贡献 code 吗？

不行 — 源代码为私有。你可以用以下方式贡献:
- 汇报可重现的 bug
- 提出跟真实工作流相关的功能建议
- Beta 期间使用并给诚实的反馈

这三种都是真实的价值。

#### 以后会开 Discord / Telegram 社群吗？

也许，某一天会。目前开了只会增加维护负担、效益不大。Beta 阶段 GitHub Issues + email 就够用。等使用者规模成长到值得社群空间时，会开。
