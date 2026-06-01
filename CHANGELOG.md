# Changelog

All notable changes to HappyPartners are documented in this file.

The format is loosely based on [Keep a Changelog](https://keepachangelog.com).
Versions follow [Semantic Versioning](https://semver.org).

## [0.6.0] — 2026-06-01

### Added — 文心一言 (ERNIE Bot) joins the lineup

Baidu's 文心一言 is now a selectable assistant, bringing the roster to 17 models side by side in one window.

### Improved — Steadier sending, status, and file uploads

This release smooths out the rough edges that showed up most on the Chinese-language assistants:

- **Messages that occasionally wouldn't send** — a handful of platforms had quietly reworked their page structure, so the controls the app relied on no longer matched. Refreshed across the board.
- **A status badge that flipped early or stuck late** — platforms with no detectable "stop" control could conclude a turn too soon (or never). These now fall back to network + text-stability detection, so the badge tracks the real state of the reply.
- **Replies that picked up a model's "thinking" text** — on assistants that render a separate reasoning section, capture sometimes grabbed the reasoning instead of the final answer. Body extraction now excludes the thinking section.

File uploads are also more reliable on the assistants that use a format-specific upload menu (document / image / audio / video), with an added safety net so an attachment no longer fails to register without warning.

### Internal — Extended dev-only diagnostics

The internal-only diagnostics layer (completely inert in the public build, no data collected) gained automatic capture of composer markup when a selector misses, plus a few hardening tweaks. Purely a maintenance aid.


## [0.5.12] — 2026-05-29

### Changed — Google sign-in overhaul: dropped the embedded-browser workaround

Google relaxed its "this browser or app may not be secure" embedded-browser detection sometime in 2026-04/05. A controlled experiment (a minimal test app embedding Gemini with zero disguise) confirmed a plain webview now logs into Gemini cleanly — no User-Agent spoofing, no client-hint rewriting, nothing.

So the entire workaround we had built up to defeat that block is now not just unnecessary but actively harmful: the fake "I'm Android Chrome 89 / desktop Chrome 146" User-Agent it injected no longer matched the honest client-hint headers once detection eased, and *that inconsistency* is exactly what trips bot-detection (it was also the root of the Cloudflare verification loops on Grok / Perplexity / Copilot).

This release removes the disguise and lets every embedded login present a single, honest, self-consistent identity:

- The Android-UA / Sec-CH-UA header rewriting for Google domains is gated behind a single `GOOGLE_AUTH_WORKAROUND` switch (default **off**). Flip it back on in one line if Google ever re-tightens.
- The desktop-Chrome User-Agent override on the Gemini / AI Studio tiles is removed — they now use the honest default UA, which still renders desktop layout.
- The OAuth popup no longer spoofs a Chrome UA either, so the popup and the tile that consumes the resulting cookie present the same identity.

Net effect: **Gemini and AI Studio sign-in work without any disguise**, and the long-standing fingerprint inconsistencies that caused intermittent Cloudflare loops are gone.

### Fixed — "Continue with Google" now works for Claude

Claude's Google sign-in never completed before — it always ended on "There was an error logging you in," forcing users onto email login. The cause was architectural, not cosmetic: Claude's OAuth hands its result back to the originating page via `window.opener.postMessage`, but the app opened auth popups as *detached* windows with no opener link, so the result could never be delivered.

Auth popups are now created as true child windows (preserving the `window.opener` relationship and inheriting the tile's session), so both postMessage-style and redirect-style OAuth flows complete. Claude "Continue with Google" works for the first time, and other providers' popup logins (Google / Apple / Microsoft / GitHub across all tiles) are more robust as a result.

### Fixed — Premature "done" on Gemini, ChatGPT, and Claude

The status badge sometimes flipped to "done" while the AI was still typing — most visibly on long answers, where the badge said finished a second after the model started. Three separate root causes, one symptom:

- **Gemini** — the platform had silently restructured its DOM, moving the `send-button` / `stop` classes onto a wrapper element. Our stop-button selector matched nothing, so streaming was never detected and the round was concluded the moment any preamble text appeared. Selectors updated to the new structure, plus a dedicated streaming indicator tied to the wrapper's generation-only `stop` class.
- **ChatGPT** — completion was gated on a `.result-streaming` element that only appears once the *answer* renders, not during the early "Thinking…" phase. While reasoning, the monitor saw no streaming indicator and ingested the 8-character preamble as the final answer. It now falls back to the stop button, which is present throughout generation.
- **Claude** — same shape as ChatGPT (its loading-background indicator is absent during early tool-use / first tokens). Streaming detection now treats the stop button *or* the loading background as "still generating," so neither early-phase gap nor an idle empty composer is misread.

Also folded in the reasoning / tool-use completion hardening (a one-tick stability confirmation after a DOM stream-end signal) so a brief pause between reasoning and answer no longer ends the round early.

### Fixed — File upload no longer fails silently

When a file upload stalled, the send path used to wait out a 30-second ceiling and then quietly press on, surfacing later as a confusing "couldn't find the send button." A stuck upload now stops with a clear, actionable message — "file upload stuck, retry or remove the attachment" — instead of a silent lock.

### Internal — Dev-only diagnostics pipeline

Added an observability layer that is **only active in internal / dev builds** and ships completely inert in the public app (gated four ways; the trace table is never even created for normal users, and no data is collected). It records metadata-only traces of send / upload / completion events, self-checks every "done" conclusion against what the page actually does next, and includes a quick in-app bug-report panel — purely a maintenance tool for diagnosing the stability issues above.


## [0.5.11] — 2026-05-01

### Fixed — External link routing now path-aware

GitHub repo links (and other non-auth content on mixed-purpose hosts) now correctly open in the system browser instead of staying inside the AI tile's webview.

The previous implementation maintained a flat whitelist of "auth provider hosts" — any URL whose hostname matched would stay in-webview, on the assumption that it was part of an OAuth login flow. The list included hosts like `github.com`, `apple.com`, and `microsoft.com` to preserve OAuth redirects routed through them. The unintended side effect: every other path on those hosts also got kept in-webview, so clicking a GitHub repo link inside a Gemini answer would silently navigate the Gemini tile away from Gemini and into GitHub, with no way back except reload.

The whitelist is now path-aware. Pure auth domains (`accounts.google.com`, `appleid.apple.com`, `login.microsoftonline.com`, etc.) still match any path — the entire host is auth, by definition. Mixed-purpose hosts (`github.com`, `apple.com`, `microsoft.com`, `openai.com`, `anthropic.com`, etc.) now only match their actual login / OAuth paths (`/login`, `/oauth/*`, `/sessions`, `/sign-in`, etc.); everything else falls through to the system-browser route.

Both layers that handle external link routing are updated together:

- **`main.js`** — `setWindowOpenHandler` (for `window.open` and `target="_blank"` links) and the `will-navigate` safety net (for SPA navigations via `window.location.href` etc.).
- **`webview-manager.js`** — the in-webview click interceptor injected into each AI page, which catches `<a href>` clicks before the platform's own JS does anything with them.

Net effect: Gemini login still works exactly as before (`accounts.google.com` is a pure auth host, all paths stay in webview). GitHub repo browsing in any AI's answer now opens in the system browser as intended. Same correction applies to Apple support pages, Microsoft docs, OpenAI marketing pages, etc. — content URLs go external; only login URLs stay in-webview.


## [0.5.10] — 2026-05-01

### Fixed — Composer file attachment UX

Fixes a cluster of related composer-attachment problems that all surfaced as the same vague symptom: "I attached a file and the send button does nothing."

- **Oversize file silent failure.** Files over the size limit reached `serializeFile()` at dispatch time and threw, but the click handler's outer try had no catch — the error became an unhandled promise rejection and the user saw nothing. Now files are pre-validated at upload (drop / paste / attach) and rejected with a visible warning. A defensive try-catch around `serializeFiles()` in the dispatch path also surfaces any error message that slips through (e.g. queue-edit replays of legacy oversize payloads).
- **Attached files cleared when switching conversations.** Drafts persist `promptText`, references, and quotes across conversation switches, but file payloads were intentionally excluded (binary blobs are too large for localStorage's 5MB limit). That decision still holds across app restarts, but losing files when switching A → B → A within the same app session was a real data-loss surprise. An in-memory `Map` now caches `File` objects per draft key so within-session switching restores them. Cleared on successful dispatch and on session deletion.
- **File size limit raised from 10MB to 20MB.** 10MB was below every major AI platform's own limit (ChatGPT 20MB, Claude 30MB, Perplexity 25MB, etc.) and rejected legitimate iPhone HEIC photos and 4K screenshots. 20MB matches the most common platform limit and keeps webview-injection latency under 5 seconds per platform.
- **Uncommon-format warning at upload.** Dragging an FBX, PSD, or similar format used to silently fail inside each AI platform's own validator with no feedback at the HappyPartners level — different platforms would inconsistently accept or reject, and the user would think "drag failed." A new advisory warning lists files whose extension isn't in the common-supported set ("may not be supported by most AI platforms"), without rejecting them — power users can still try edge-case formats their target AI might handle.
- **Multiple warnings combine instead of overwriting.** Status bar previously showed only the latest warning. Dragging "FBX + 25MB PSD" rendered just the format warning; the size warning that filtered out the PSD was clobbered by the format warning that fired immediately after. All file warnings now collect into a single combined message, separated by ` / `.
- **Stale format warnings auto-clear when the file is removed.** Removing the chip for a file that triggered the format warning now re-runs validation and clears the warning if no uncommon files remain — instead of leaving "model.fbx may not be supported" sitting in the status bar after the FBX is gone.
- **Long filenames truncated in status messages.** A 50-character filename used to push the actual error info (size, limit, suggestion) off the visible status bar. Filenames over 20 characters now display as `head…ext.ext` (e.g. `Los-York-Studio….psd`), so the size, limit, and "please use a smaller file" suggestion stay visible.

The cumulative effect: file attachment problems now show a clear, single-line message that names the file (truncated), states the size, names the limit, and suggests a fix — instead of a silent failure that looks like a broken send button.


## [0.5.9] — 2026-04-28

### Fixed — IME composition no longer breaks Enter-key inputs

CJK users (Chinese, Japanese, Korean) typing through an input method (注音, 拼音, IME) press Enter to commit a candidate from the IME's suggestion popup. Several keydown handlers across the app were intercepting Enter without checking for active IME composition, so picking a candidate would also fire the handler's "submit" action — closing the modal, committing a rename, or inserting a line break before the user actually finished typing the character.

Fixed in four places:

- **Nickname modal** (first-launch + sidebar edit). Pressing Enter to pick a Chinese candidate no longer closes the modal mid-composition. Previously this affected every CJK user on first launch.
- **Sidebar inline rename** (project + session). Renaming with Chinese input now lets the IME consume Enter for candidate selection without committing the rename prematurely.
- **Custom prefix editor** (Templates → prefix settings). Enter for picking a CJK candidate no longer triggers a line-break insertion before the candidate is committed.
- **Dev console password input**. Same guard for consistency, even though passwords rarely use CJK input.

The standard fix in all four locations: check `e.isComposing` (and the legacy `keyCode === 229` fallback) at the top of the keydown handler and return early. The composer input already had this guard from earlier work — these four sites simply weren't covered when added incrementally.


## [0.5.8] — 2026-04-25

### Fixed — Composer hotkeys no longer hijack normal text
- **`@` mention picker triggered mid-word.** Typing an email like
  `eric@gmail.com` would pop the AI mention picker on the `@`, breaking
  the input flow. The `@` hotkey now only triggers when it starts a new
  word (start of input, after whitespace, or after a newline) — matching
  the convention in Slack, Discord, Notion, Linear, GitHub, and Twitter.
- **`\` quote picker triggered inside file paths.** Same fix applied:
  `\` only opens the quote picker at a word boundary, so paths like
  `C:\Users\foo` are typed as plain text.
- **`/` template picker triggered inside URLs.** Stricter than `@` and
  `\` — `/` only triggers at the start of a line (start of input or
  after a newline). URLs like `https://...` and phrases like `and/or`
  are typed as plain text instead of opening the template picker.


## [0.5.7] — 2026-04-24

### Fixed — Claude Copy All (two regressions)
- **Wrong-message extraction in historical Claude conversations.** Claude
  silently renamed its assistant-message class from `.font-claude-message`
  to `.font-claude-response`; our message container selector stopped matching
  and Copy All fell through to a "pick the longest text in the whole
  conversation" fallback — which usually returned a mid-conversation reply
  instead of the latest turn. The selector now accepts both class names so
  a future roll-back won't break us again.
- **Code blocks silently dropped from Copy All.** Claude wraps code-block
  containers in a Tailwind group class (`group/copy`) that enables the
  hover-reveal copy button. Our exclude list had a broad `[class*='copy']`
  pattern that accidentally stripped the entire code block — users copying
  Claude replies containing fenced code got responses with the prose but
  none of the code. The exclude list now targets buttons specifically and
  no longer uses substring wildcards that collide with Tailwind's
  `group/NAME` convention.

### Changed
- The "only extract the latest container, never walk back" rule now applies
  universally to avoid masking genuine empty-latest states (previously the
  extractor would silently substitute an older turn's text when the latest
  turn yielded no extractable content — e.g. an Artifact-only response).
  Empty-latest correctly falls back to stored SQLite materials instead.


## [0.5.5] — 2026-04-24

### Changed
- **Copy All rewritten to live-extract DOM.** No longer depends on the
  done-detection signal — what you see on screen is what you get.
- **Copy All historical conversation fallback.** When live DOM has no
  content (e.g. loaded past session, webview not on original URL),
  falls back to stored SQLite materials so you still get the response.

### Added
- **Claude Artifact detection.** Side panel content extracted when
  open; closed panel surfaces a status-line hint instead of polluting
  the copied text.
- **Toolbar safety timeout.** Copy / Download buttons now auto-recover
  after 15 seconds if a webview call hangs.

### Fixed
- **Gemini UI chrome excluded** from extracted text (顯示思路, Gemini said,
  +N citations, sources list, footnote superscripts).
- **ChatGPT mid-stream partial captures.** STREAM_END signal now
  re-probes after 1.5s to confirm streaming actually stopped, so
  transient DOM mutations during reasoning transitions no longer
  trip a false "done".

## [0.5.4] — 2026-04-23

### Fixed
- **Cmd+R in Focus mode** now only reloads the focused AI, not all 4 visible AIs. Background AIs mid-generation are no longer killed when you reload the one you're currently looking at. Normal multi-pane mode behavior unchanged.


## [0.5.3] — 2026-04-22

### Fixed
- **Completion detection**: ChatGPT o-series reasoning pauses and Claude tool-use windows no longer falsely mark the AI as "Done". A 2-tick confirmation now guards against transient idle states — the platform must be idle for two consecutive checks before the response bank closes the round.
- **Claude content extraction**: The message container selector is now strictly scoped to AI-response blocks. This prevents the "Copy all" action from accidentally capturing the user's own message when Claude is still in a tool-use pause.


## [0.5.2] — 2026-04-22

### Added — License & Waitlist
- Self-serve license admin panel at `license.happypartners.app/admin`.
- Batch-select UI for issuing multiple license keys in a single click.
- Waitlist sign-ups now receive a confirmation email with expectations for key delivery.
- Public `/waitlist/count` endpoint with a display floor (prevents cold-start signalling of "0 / 200" on launch day).
- License emails now point to the latest DMG via `latest-mac.yml`. No manual version updates required.

### Added — Release automation
- `npm run ship` — interactive version bump (patch / minor / major) + build + notarize + optional R2 upload.
- `npm run upload` — upload an already-built version to R2.
- `npm run deploy:web` — push the marketing website to Cloudflare Pages.

### Improved — Ready probe UX
- Composer status bar now clears the "loading for over Xs" warning the moment an AI finishes loading — no more stale warnings.
- Cmd+R correctly resets probe timers across all platforms.
- Platform-not-ready events now emit unconditionally, so the probe resets even when the platform hadn't reached ready state in the first place.

### Improved — Analytics
- Drag-relay events now distinguish "single" (drop on one AI) vs "broadcast" (drop on the banner). Broadcast counts as one event per user action, not one per target AI.
- Focus mode usage is now tracked (was previously blocked by an incomplete server allow-list).

### Fixed
- Dev app: Settings → About now shows the correct version from `package.json` (previously fell back to a hardcoded value).

### Website
- Redesigned comparison section into a 3-row layout with short section dividers.
- Updated copy: "Less shuffling. More collaboration." anchors the new narrative.
- New hero screenshot (1440p native).
- Deployed to Cloudflare Pages; custom domain bound.


## [0.5.1] — 2026-04-21

### Added — Distribution & updates
- Auto-update: electron-updater with differential zip updates over Cloudflare R2.
- Notarized DMG signed with a Developer ID certificate, Apple ticket stapled.
- End-to-end upgrade flow validated from a previous installation.

### Added — Email infrastructure
- `hello@happypartners.app` relay via Cloudflare Email Routing + Resend SMTP.
- All outbound product email (license keys, waitlist confirmations) sent from a unified sender address.


## [0.5.0] — 2026-04

Initial Beta release.

- 16 AI platforms (ChatGPT, Claude, Gemini, Grok, Perplexity, Copilot, AI Studio, Mistral, DeepSeek, Kimi, Qwen, Yuanbao, Doubao, MiMo, Z.ai, ChatGLM)
- Multi-pane workspace (up to 4 AIs on screen, 1×1 / 1×2 / 1×3 / 1×4 / 2×2 layouts)
- Shared composer with `@` target selection, `\` quote selection, `/` prompt templates
- Alt + drag cross-AI relay (single and broadcast)
- Projects, conversation history, Chinese-friendly FTS5 full-text search
- Trilingual UI: 繁體中文, 简体中文, English
- Signed + notarized macOS DMG, Apple Silicon
