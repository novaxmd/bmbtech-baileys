# 1.2.0 (2026-07-08)

### Features
- Added a proper username API — `checkUsername`, `setUsername`, `deleteUsername`, `getMyUsername`, `setUsernamePin`, `findUserByUsername`, `fetchContactUsernames`, `checkUsernameMulti`, `getUsernameRecommendations`
- Added `USyncPictureProtocol` and `USyncTextStatusProtocol` for fetching profile pictures and text statuses via USync
- Added `AIRich`, `Button`, `ButtonV2`, `Carousel`, and `Toolkit` (`WABuilder` module) — rich message builders for interactive buttons, carousels, and Meta AI-style responses with inline citations, hyperlinks, LaTeX, code blocks, and tables
- Added `sendInteractive` (also callable as `inappsignup` / `inapp_signup`) — sends interactive buttons, but automatically falls back to plain text when the bot's paired device is iOS
- Added shorter import aliases: `AIRich` as `RichMessage` / `Rich` / `RichMsg` / `RichAI`, `Button` as `Buttons` / `Btns`, `ButtonV2` as `ButtonsV2` / `BtnsV2` / `NewButtons`
- Added `host` option to `downloadContentFromMessage` for overriding the media host manually
- Added `groupOnlineCount` to presence updates when WhatsApp includes it
- Added `destroy()` on the event buffer, actually clearing timers and listeners on socket end (previously called but never implemented)

### Fixes
- Confirmed protection against the message-spoofing/app-state-corruption issue from GHSA-qvv5-jq5g-4cgg (CVE-2026-48063) — spoofed history-sync, app-state-key-share, and placeholder-resend-response payloads not sent from your own account are dropped
- `generateProfilePicture` now crops from the center instead of the top-left corner, so non-square photos no longer come out lopsided
- App state sync no longer throws and kills the whole sync on a single bad patch or a hash mismatch — it now logs a warning and continues
- LID mapping store rewritten to batch and coalesce concurrent lookups, plus a proper `close()`
- Dropped spoofed "self-only" protocol messages (history sync notifications, app state key shares, etc.) that don't actually come from `fromMe`
- `getChatId` now throws a clear error on missing `remoteJid`/`participant` instead of silently working with `undefined`
- `profilePictureUrl` querying refactored to the newer, simpler `buildProfilePictureQueryContent` approach
- Dependency versions and resolutions synced with upstream Baileys

# 1.1.0 (2026-06-04)

### Features
- Added `registerSocketEndHandler` — register async callbacks that fire when the socket closes
- Added `fetchAccountReachoutTimelock()` — query account restriction/timelock status from WA servers
- Added `fetchNewChatMessageCap()` — query new chat message quota and usage
- Added `buildPairingQRData` and `getCompanionPlatformId` from companion-reg-client-utils — QR now encodes companion platform type for proper device recognition
- Exported `XWAPaths`, `QueryIds`, `ReachoutTimelockEnforcementType`, `ReachoutTimelockState`, `NewChatMessageCapInfo`, `NewChatMessageCappingStatusType`, `NewChatMessageCappingMVStatusType`, `NewChatMessageCappingOTEStatusType` types
- Added `'message-capping.update'` event type

### Fixes
- Fixed pre-key upload: deduplication via in-flight promise, retry logic now internal to `uploadLogic`, server drives timing
- Fixed `ev.destroy()` on socket close to prevent memory leaks
- Fixed `signalRepository.close()` called on socket end
- Fixed socket end handlers run before `ev.destroy()` — guarantees all post-close cleanup runs in order
- Fixed newsletter join endpoint to `xwa2_newsletter_join_v2` and leave to `xwa2_newsletter_leave_v2`
- Fixed group metadata: `getBinaryNodeChild` error now surfaces WA server error code instead of crashing
- Fixed album messages: `contextInfo` now passed to each album item, `userJid` uses socket auth state
- Fixed `handleGroupStory`: removed stale fallback chain, now directly calls `generateWAMessageContent`
- Fixed `handleEvent`: `userJid` now from socket auth state instead of generated fake JID
- Fixed `handlePollResult`: `userJid` now from socket auth state
- Fixed `detectType` to accept `content.album` as alias for `content.albumMessage`
- Fixed `bmbHandler` constructor: removed unused `utils` parameter
- Fixed `getCompanionPlatformId` replacing deprecated `getPlatformId` for companion platform registration

# 1.0.10 (2025-05-03)

Initial release of bmb-baileys
