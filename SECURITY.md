# Security Policy

**Language:** [English](#english) · [繁體中文](#zh-tw) · [简体中文](#zh-cn)

---

<h2 id="english">English</h2>

Thank you for helping keep HappyPartners and its users safe.

### Reporting a vulnerability

**Please do not open a public GitHub issue for security reports.**

Instead, send an email to **hello@happypartners.app** with the subject line prefixed `[SECURITY]`. Include:

- A clear description of the issue
- Steps to reproduce (if applicable)
- The version of HappyPartners you were using
- Your macOS version
- Any proof-of-concept code, logs, or screenshots

### What to expect

HappyPartners is a Beta product maintained by one person. Response times are best-effort:

- **Acknowledgement**: within 72 hours
- **Initial triage**: within 1 week
- **Fix timeline**: depends on severity — critical issues prioritized, non-critical issues rolled into a regular patch release

If you do not receive a response within 72 hours, please send a follow-up email. Reports are never ignored, but a human occasionally misses a notification.

### Scope

The following are in scope for security reports:

- **The HappyPartners macOS application** (license handling, webview isolation, IPC, file upload paths, local storage, analytics transport)
- **Backend services** under `*.happypartners.app` (license worker, waitlist API, downloads CDN)
- **Website** (`happypartners.app`)

Out of scope:

- Third-party AI platforms (report those to the respective vendors: OpenAI, Anthropic, Google, etc.)
- Issues that require physical access to an unlocked Mac
- Social engineering of the maintainer
- Rate-limiting or denial-of-service reports against public endpoints

### Coordinated disclosure

Please do not publicly disclose the vulnerability until:

1. A fix has been released, or
2. 90 days have passed since the report (whichever comes first)

After resolution, we're happy to credit you in the release notes if you'd like.

---

<h2 id="zh-tw">繁體中文</h2>

感謝你幫助保護 HappyPartners 和使用者的安全。

### 回報安全漏洞

**請勿在 GitHub 上開 public issue 回報安全問題。**

請改用 email 寄到 **hello@happypartners.app**，主旨前面加上 `[SECURITY]` 標記。內容請包含：

- 清楚描述問題
- 重現步驟（若適用）
- 你使用的 HappyPartners 版本
- macOS 版本
- Proof-of-concept 程式碼、log、或截圖

### 預期處理時程

HappyPartners 是一款獨立開發者維護的 Beta 產品，回覆時程盡力而為：

- **確認收到**：72 小時內
- **初步分析**：一週內
- **修復時程**：依嚴重度決定 — 關鍵漏洞優先處理，一般問題併入常規 patch release

若 72 小時內沒收到回覆，請再寄一次提醒。我們絕不會忽略回報，但偶爾會漏看通知。

### 範圍

以下項目屬於安全回報範圍：

- **HappyPartners macOS 應用程式**（license 處理、webview 隔離、IPC、檔案上傳路徑、本地儲存、分析傳輸）
- **`*.happypartners.app` 後端服務**（license worker、waitlist API、下載 CDN）
- **官網**（`happypartners.app`）

不在範圍內：

- 第三方 AI 平台（請向該廠商回報：OpenAI、Anthropic、Google 等）
- 需要實體接觸到已解鎖 Mac 才能重現的問題
- 對維護者的社交工程
- 對 public endpoint 的限流 / DDoS 回報

### 協同揭露（coordinated disclosure）

在下列情況發生前請勿公開揭露漏洞：

1. 修復已發布，或
2. 回報後已滿 90 天（兩者取先者）

漏洞修復後，若你願意，我們會在 release notes 裡標記你的貢獻。

---

<h2 id="zh-cn">简体中文</h2>

感谢你帮助保护 HappyPartners 和用户的安全。

### 汇报安全漏洞

**请勿在 GitHub 上开 public issue 汇报安全问题。**

请改用 email 寄到 **hello@happypartners.app**，主题前面加上 `[SECURITY]` 标记。内容请包含:

- 清楚描述问题
- 重现步骤（若适用）
- 你使用的 HappyPartners 版本
- macOS 版本
- Proof-of-concept 代码、log、或截图

### 预期处理时程

HappyPartners 是一款独立开发者维护的 Beta 产品，回覆时程尽力而为:

- **确认收到**：72 小时内
- **初步分析**：一週内
- **修复时程**：依严重度决定 — 关键漏洞优先处理，一般问题并入常规 patch release

若 72 小时内没收到回覆，请再寄一次提醒。我们绝不会忽略汇报，但偶尔会漏看通知。

### 范围

以下项目属于安全汇报范围:

- **HappyPartners macOS 应用程序**（license 处理、webview 隔离、IPC、文件上传路径、本地储存、分析传输）
- **`*.happypartners.app` 后端服务**（license worker、waitlist API、下载 CDN）
- **官网**（`happypartners.app`）

不在范围内:

- 第三方 AI 平台（请向该厂商汇报：OpenAI、Anthropic、Google 等）
- 需要实体接触到已解锁 Mac 才能重现的问题
- 对维护者的社交工程
- 对 public endpoint 的限流 / DDoS 汇报

### 协同揭露（coordinated disclosure）

在下列情况发生前请勿公开揭露漏洞:

1. 修复已发布，或
2. 汇报后已满 90 天（两者取先者）

漏洞修复后，若你愿意，我们会在 release notes 里标记你的贡献。
