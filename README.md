# DJX Systems NVR AI Agent Skills

🎥 **Pull Hikvision & Dahua CCTV footage by just *asking*.** Tell your AI agent *"grab the front-door camera from 8 to 9 PM last night"* and get a clean, verified MP4 back — no clunky web UI, no channel-guessing, no hand-built ffmpeg commands.

[Website (catalog)](https://djxsystems.com/skills) · [Hikvision skill](https://djxsystems.com/skills/hikvision-nvr) · [Dahua / IC Realtime skill](https://djxsystems.com/skills/dahua-nvr) · [Agensi (Hikvision)](https://www.agensi.io/skills/hikvision-nvr) · [Technical guide](https://djxsystems.com/insights/hikvision-nvr-time-range-download)

> **This repository does NOT contain the paid skill packages.** It provides public documentation, examples, compatibility notes, and safe sample outputs only.

---

## The two paid skills

### Hikvision NVR AI Agent Skill

Ask your AI agent in plain English for a camera and a time window — *"pull the front-door camera from 8 to 9 PM last night"* — and get back a clean, verified MP4. Built for Hikvision DVRs/NVRs and OEM rebrands that expose ISAPI, with an RTSP playback fallback. Tested on four recorder models spanning firmware V3.1.10 through V4.50.000.
→ [Hikvision product page](https://djxsystems.com/skills/hikvision-nvr) · [docs/hikvision-nvr.md](docs/hikvision-nvr.md)

### Dahua / IC Realtime NVR AI Agent Skill

The same workflow for the Dahua-protocol family: Dahua, IC Realtime, newer Lorex, Amcrest, Honeywell Performance, the EZVIZ business line, and other Dahua OEM rebrands. Tested across multiple Dahua-family firmware versions; there is no fixed model matrix. (Older Lorex models use a different protocol and are not supported.)
→ [Dahua product page](https://djxsystems.com/skills/dahua-nvr) · [docs/dahua-ic-realtime-nvr.md](docs/dahua-ic-realtime-nvr.md)

## Who this is for

- Security teams that export evidence/review clips often enough for the manual process to hurt.
- IT and security integrators supporting several Hikvision, Dahua, or OEM recorder models.
- Property managers and businesses that need a non-technical person to request footage by name and time.
- Developers building agent workflows around recorded CCTV footage.
- Anyone who needs repeatable, scriptable NVR clip exports.

## Supported agent environments

Built on the open Agent Skills (`SKILL.md`) standard — works with any agent that can read a `SKILL.md` skill and run its scripts: Claude Code & Desktop, OpenAI Codex, Cursor, Cline, Aider, Gemini CLI, and more. Verified hands-on with Claude and Codex.

**Prerequisites:** Python 3.10+, with `ffmpeg` and `ffprobe` on PATH.

See [docs/compatibility.md](docs/compatibility.md) for recorder model and firmware details.

## What the paid skills automate

You ask an AI agent in plain English for a camera and a time window, and the skill:

- Locates the recorded footage on the registered recorder and downloads it.
- Joins multi-segment recordings into a single continuous file.
- Converts to a standard MP4 (faststart, browser-ready) and trims to the requested window.
- Verifies the result with `ffprobe` (codec, resolution, frame rate, duration), plus optional on-screen-timestamp OCR when Pillow + tesseract are present.
- Emits NDJSON progress events so the download can run as a background task while your agent stays responsive.
- Registers recorders by short alias, so prompts use names instead of IP addresses.
- Runs a strategy ladder with backoff/retry and an RTSP fallback, recovering the full requested window even when a fast download comes back short.
- Guides onboarding with secure credential entry and can install `ffmpeg` with your confirmation.
- Writes output in flexible layouts (flat, per-day, per-camera, or per-day-per-camera).

**Honest caveats:**

- **Trimming is keyframe-approximate** — the start is accurate to within about one to three seconds, not frame-exact. This is review-grade, not an evidence-grade export tool.
- **Audio is not included** — it is dropped during conversion.
- **Credentials** are stored in your OS keyring (Windows Credential Manager / macOS Keychain / Linux Secret Service) or a local `.env` file (restricted to your user on macOS/Linux); they never appear in command lines or logs.
- The skill **accepts a recorder's self-signed certificate** over HTTPS — keep recorders on a LAN or VPN, not the public internet.
- **No telemetry, no auto-update, never executes downloaded code.** A single auth attempt per method avoids account lockouts.

More detail in [docs/security.md](docs/security.md).

## What this repo does NOT include

This is documentation, examples, and linkage only — you cannot reconstruct the paid skill from it. It deliberately omits:

- The paid `SKILL.md` text.
- Any implementation or source-code scripts.
- Protocol logic — the specific recorder endpoints, search and download strategy, format-conversion internals, and authentication details that make the skills work.
- Credential-handling code.
- Any attack, bypass, or recorder-exploitation tooling.

Sample outputs under [samples/](samples/README.md) are illustrative shapes only — generic field names and structure with obviously fictional placeholder values.

## Get the skills

One-time purchase, **$29 each** (regularly $39), or **$50 for the bundle** of both. Licensed under the PolyForm Internal Use License 1.0.0 (not open source).

- Buy on the catalog: [djxsystems.com/skills](https://djxsystems.com/skills)
- Hikvision on Agensi: [agensi.io/skills/hikvision-nvr](https://www.agensi.io/skills/hikvision-nvr)
- The Dahua / IC Realtime skill is also coming to Agensi.

Not sure your recorder is a fit? Send the brand, model, and firmware through the [contact form](https://djxsystems.com/contact) for a pre-purchase sanity check.

See [docs/listing-kit.md](docs/listing-kit.md) for short copy and links suitable for catalogs and directories.

## Report compatibility

If you run a recorder we haven't listed, a compatibility report helps everyone. Open an issue using the templates under [.github/ISSUE_TEMPLATE/](.github/ISSUE_TEMPLATE/) — please include brand, model, firmware version, and what worked or failed. You can also reach us through the [contact form](https://djxsystems.com/contact).

## Links

- [docs/hikvision-nvr.md](docs/hikvision-nvr.md) — Hikvision skill overview and details
- [docs/dahua-ic-realtime-nvr.md](docs/dahua-ic-realtime-nvr.md) — Dahua / IC Realtime skill overview and details
- [docs/compatibility.md](docs/compatibility.md) — supported brands, models, and firmware
- [docs/security.md](docs/security.md) — credential storage, network posture, and safety
- [docs/example-prompts.md](docs/example-prompts.md) — plain-English prompts that work
- [docs/listing-kit.md](docs/listing-kit.md) — copy and links for catalogs and directories
- [samples/README.md](samples/README.md) — illustrative sample outputs
- [Technical guide](https://djxsystems.com/insights/hikvision-nvr-time-range-download) — the manual workflow these skills automate
- [Agent Skills (`SKILL.md`) standard](https://agentskills.io)
