# Listing Kit

For directory and catalog maintainers. This page gives you canonical product
names, ready-to-use descriptions, tags, categories, links, and contact details
so the two DJX Systems NVR AI Agent Skills are listed accurately. Everything here
is cleared for public reuse.

If anything is unclear or you need a different format, reach out through the
[contact form](https://djxsystems.com/contact).

- **Repo overview:** [../README.md](../README.md)
- **Hikvision skill details:** [docs/hikvision-nvr.md](./hikvision-nvr.md)
- **Dahua / IC Realtime skill details:** [docs/dahua-ic-realtime-nvr.md](./dahua-ic-realtime-nvr.md)
- **Compatibility:** [docs/compatibility.md](./compatibility.md)

---

## Publisher

| Field | Value |
| --- | --- |
| Publisher | DJX Systems |
| Website | https://djxsystems.com |
| Catalog | https://djxsystems.com/skills |
| Contact | https://djxsystems.com/contact |
| Standard | Open Agent Skills (`SKILL.md`) — https://agentskills.io |
| License | PolyForm Internal Use License 1.0.0 (paid product, not open source) |

**Affiliation note for listings:** DJX Systems is independent and unaffiliated
with Hikvision, Dahua, IC Realtime, or any other recorder manufacturer.
Hikvision, Dahua, IC Realtime, Lorex, Amcrest, Honeywell, EZVIZ, and other brand
names are trademarks of their respective owners.

---

## Product 1 — Hikvision NVR AI Agent Skill

| Field | Value |
| --- | --- |
| Canonical name | Hikvision NVR AI Agent Skill |
| Short name | Hikvision NVR |
| Category | Security & Surveillance |
| Price | $29 one-time (regularly $39) |
| Product page | https://djxsystems.com/skills/hikvision-nvr |
| Also listed on | Agensi — https://www.agensi.io/skills/hikvision-nvr |
| Public docs | [docs/hikvision-nvr.md](./hikvision-nvr.md) |

**One-line description**

> Ask your AI agent in plain English for a Hikvision camera and a time window —
> get back a clean, verified MP4.

**Short description (≈ 2 sentences)**

> Getting last night's footage off a Hikvision DVR or NVR usually means fighting
> a clunky web interface, guessing channel numbers, and hand-building ffmpeg
> commands. Skip it: tell your AI agent "pull the front-door camera from 8 to 9
> PM last night" and get back a clean, browser-ready MP4 that's been verified
> against the time you asked for.

**Paragraph description**

> The Hikvision NVR AI Agent Skill turns recorded-footage exports into a plain
> sentence. You register each recorder once with a short alias, then ask your AI
> agent for a camera and a time window in ordinary language. The skill locates
> the recording over Hikvision's ISAPI interface (with an RTSP playback
> fallback), downloads it, joins multi-segment recordings, converts the result to
> a standard faststart MP4, trims it to your window, and verifies the output with
> ffprobe — optionally checking the on-screen-display timestamp too. Built for
> security teams, integrators, and developers who export review clips often
> enough that the manual process hurts. Tested hands-on across eight recorder
> models spanning firmware V3.1.10 through V4.50.000.

**Supported recorders:** Hikvision DVRs/NVRs and OEM rebrands that expose ISAPI.
Tested hands-on across eight Hikvision recorder models spanning firmware
V3.1.10–V4.50.000: DS-7316HQHI-SH, DS-7616NI-E2/16P, DS-7716NI-SP/16,
DS-7716NI-I4/16P, DS-7732NI-I4/16P, DS-9016HWI-ST, DS-9632NI-ST, DS-9632NI-I8.

---

## Product 2 — Dahua / IC Realtime NVR AI Agent Skill

| Field | Value |
| --- | --- |
| Canonical name | Dahua / IC Realtime NVR AI Agent Skill |
| Short name | Dahua / IC Realtime NVR |
| Category | Security & Surveillance |
| Price | $29 one-time (regularly $39) |
| Product page | https://djxsystems.com/skills/dahua-nvr |
| Also listed on | Agensi — https://www.agensi.io/skills/dahua-ic-realtime-nvr |
| Public docs | [docs/dahua-ic-realtime-nvr.md](./dahua-ic-realtime-nvr.md) |

**One-line description**

> Ask your AI agent in plain English for a camera and a time window — get back a
> clean, verified MP4 from your Dahua, IC Realtime, newer Lorex, Amcrest, or
> Honeywell recorder.

**Short description (≈ 2 sentences)**

> Pulling recorded video off a Dahua or IC Realtime recorder usually means a
> clunky web interface and a fiddly app. Skip it: tell your AI agent "pull the
> lobby camera from 8 to 9 PM yesterday" and get back a clean, verified MP4 —
> works across the whole Dahua-protocol family.

**Paragraph description**

> The Dahua / IC Realtime NVR AI Agent Skill brings the same plain-English
> workflow to the Dahua-protocol family of recorders. You register each recorder
> once with a short alias, then ask your AI agent for a camera and a time window.
> The skill searches the recorder over the Dahua HTTP CGI interface, downloads the
> segment, converts it into a standard faststart MP4 regardless of the recorder's
> native format, joins multi-segment recordings, trims to your window,
> and verifies the result with ffprobe (and the on-screen-display timestamp when
> the optional OCR tools are present). It includes a strategy ladder with
> backoff/retry and an RTSP fallback. Tested across multiple Dahua-family firmware
> versions; there is no fixed model matrix yet.

**Brands it works with:** Dahua, IC Realtime, newer Lorex, Amcrest, Honeywell
Performance, the EZVIZ business line, and other Dahua OEM rebrands that expose the
same CGI/RTSP interface.

> **Listing accuracy note:** older Lorex models use a different protocol and are
> **not** supported. Please do not list this skill as supporting all Lorex
> recorders — only newer, Dahua-protocol Lorex models.

---

## The bundle

| Field | Value |
| --- | --- |
| Name | Hikvision + Dahua / IC Realtime NVR Skills Bundle |
| Price | $50 one-time (both skills) |
| Where | https://djxsystems.com/skills |

Both skills are sold separately at $29 each, or together as a $50 bundle. They
pair cleanly for anyone running a mixed Hikvision + Dahua fleet.

---

## What both skills do (shared summary)

A user asks an AI agent in plain English for a camera and a time window. The
skill:

- Locates the recorded footage on the registered recorder and downloads it.
- Joins multi-segment recordings into one continuous file.
- Converts to a standard MP4 (faststart, browser-ready) and trims to the window.
- Verifies the result with `ffprobe` (codec, resolution, frame rate, duration),
  plus optional on-screen-timestamp OCR when Pillow + tesseract are present.
- Emits NDJSON progress events so a download can run as a background task.
- Registers recorders by short alias, so prompts use names instead of IPs.
- Runs a strategy ladder with backoff/retry and an RTSP fallback.
- Guides onboarding with secure credential entry and can install `ffmpeg` (with
  confirmation).
- Writes output in flexible layouts (flat, per-day, per-camera, per-day-per-camera).

**Honest caveats to carry into any listing:**

- Trimming is keyframe-approximate — start accurate to within about 1–3 seconds,
  not frame-exact. Review-grade, not an evidence-grade export tool.
- Audio is not included (dropped during conversion).
- Credentials are stored in the OS keyring (Windows Credential Manager / macOS
  Keychain / Linux Secret Service) or a local `.env`; never in command lines or
  logs. (Do not describe this as "encrypted at rest.")
- No telemetry, no auto-update, never executes downloaded code.

---

## Agent compatibility (use this wording exactly)

> Built on the open Agent Skills (`SKILL.md`) standard — works with any agent
> that can read a `SKILL.md` skill and run its scripts: Claude Code & Desktop,
> OpenAI Codex, Cursor, Cline, Aider, Gemini CLI, and more. Verified hands-on
> with Claude and Codex.

Please keep the distinction intact when you list these:

- **Claude (Code & Desktop) and OpenAI Codex** — *verified hands-on*.
- **All other `SKILL.md`-compatible agents** — *work via the open standard*; not
  separately tested. Do not claim every agent was tested, and do not use the word
  "natively" for agents beyond Claude and Codex.

**Prerequisites:** Python 3.10+, with `ffmpeg` and `ffprobe` on PATH. Runs on
Windows, macOS, and Linux. See [docs/compatibility.md](./compatibility.md).

---

## Categories and tags

**Primary category:** Security & Surveillance

**Suggested categories / collections:** AI Agents · Agent Skills · Video Tools ·
IT & Security · Developer Tools

**Shared tags**

```
security, surveillance, cctv, nvr, dvr, security-cameras, video, video-export,
mp4, rtsp, ffmpeg, agent-skills, skill-md, claude-code, openai-codex, cursor,
gemini-cli, ai-agents
```

**Hikvision-specific tags**

```
hikvision, isapi, oem-rebrand
```

**Dahua-specific tags**

```
dahua, ic-realtime, lorex, amcrest, honeywell, ezviz, cgi, dahua-oem
```

---

## Links (all verified to resolve)

| Purpose | URL |
| --- | --- |
| Catalog / buy | https://djxsystems.com/skills |
| Hikvision product page | https://djxsystems.com/skills/hikvision-nvr |
| Dahua / IC Realtime product page | https://djxsystems.com/skills/dahua-nvr |
| Hikvision on Agensi | https://www.agensi.io/skills/hikvision-nvr |
| Dahua / IC Realtime on Agensi | https://www.agensi.io/skills/dahua-ic-realtime-nvr |
| Technical guide | https://djxsystems.com/insights/hikvision-nvr-time-range-download |
| Contact | https://djxsystems.com/contact |
| Agent Skills standard | https://agentskills.io |

The Dahua / IC Realtime skill is **also listed on Agensi**:
https://www.agensi.io/skills/dahua-ic-realtime-nvr

---

## Logo and branding

The DJX Systems logo is available on the company site at
[djxsystems.com](https://djxsystems.com) (a square mark is used as the site/skill
icon). If you need a specific size, format, or a transparent version for a
directory listing, request it through the [contact form](https://djxsystems.com/contact)
and we'll send appropriate assets.

**Display names:** "DJX Systems" for the publisher; the canonical product names
above for each skill. Please avoid abbreviating the skill names in a way that
drops the recorder brand (e.g. keep "Hikvision" and "Dahua / IC Realtime" in the
title).

---

## Pricing summary

| Item | Price | Notes |
| --- | --- | --- |
| Hikvision NVR AI Agent Skill | $29 | Regularly $39. One-time purchase. |
| Dahua / IC Realtime NVR AI Agent Skill | $29 | Regularly $39. One-time purchase. |
| Bundle (both skills) | $50 | One-time purchase. |

License: PolyForm Internal Use License 1.0.0. This is a paid product, not open
source.

---

## Contact

For listing questions, asset requests, corrections, or a pre-purchase recorder
fit check (send the brand, model, and firmware): use the
[DJX Systems contact form](https://djxsystems.com/contact).
