# Changelog

All notable changes to HappyPartners are documented in this file.

The format is loosely based on [Keep a Changelog](https://keepachangelog.com).
Versions follow [Semantic Versioning](https://semver.org).

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
