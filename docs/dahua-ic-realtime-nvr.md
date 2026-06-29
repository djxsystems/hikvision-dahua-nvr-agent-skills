# Dahua / IC Realtime NVR AI Agent Skill

Pull recorded footage off a Dahua-family DVR or NVR by asking your AI agent in
plain English — no clunky web interface, no fiddly app, no guessing channel
numbers.

> "Pull the lobby camera from 8 to 9 PM yesterday and save it as an MP4."

That sentence is the whole interface. The skill finds the recording, downloads
it, converts it to a clean browser-ready MP4, trims it to your window, and
verifies the result before reporting success.

- **Buy / catalog:** https://djxsystems.com/skills
- **Dahua product page:** https://djxsystems.com/skills/dahua-nvr
- **Also listed on Agensi:** https://www.agensi.io/skills/dahua-ic-realtime-nvr
- **Run Hikvision recorders too?** See the companion
  [Hikvision NVR skill](./hikvision-nvr.md).
- **Back to repo overview:** [../README.md](../README.md)

---

## The pain it removes

Getting recorded video off a Dahua or IC Realtime recorder usually means logging
into a clunky web interface, guessing channel numbers, fighting a slow app, and
hoping the export actually covers the time window you wanted. Worse, the
Dahua-protocol family is fragmented: different firmware uses different container
formats, different auth schemes, and different encoding rules, so a recipe that
works on one recorder fails on the next.

This skill hands that whole mess to an AI agent. You register each recorder once
with a short alias, and from then on a request is just a sentence. The agent
locates the footage, picks the best download strategy for that recorder's
firmware, clips to the requested window, and confirms the output is real video
of plausible length — while reporting progress as it goes.

## Supported brands and recorders

This skill covers the **Dahua-protocol family** — Dahua and the many brands
built on Dahua's HTTP CGI + RTSP interface:

- **Dahua** (DH-NVR4xxx / 5xxx, DH-XVR series, and similar)
- **IC Realtime** (ICIP-, NVR-, MAX-IP series)
- **Lorex — newer models only.** Older Lorex recorders use a different protocol
  and are **not supported.**
- **Amcrest** (NV21xx, NV41xx, etc.)
- **Honeywell Performance** (HEN, HQA series)
- **EZVIZ business line**
- Other Dahua OEM rebrands that expose the same CGI / RTSP interface

Non-Dahua brands (Hikvision, UniFi Protect, Axis) are not covered here. If your
cameras run on Hikvision, use the [Hikvision NVR skill](./hikvision-nvr.md)
instead.

**Tested across multiple Dahua-family firmware versions**, including older IC
Realtime firmware and newer Dahua firmware with stricter encoding requirements.
There is no fixed model matrix yet — Dahua-protocol recorders that expose CGI
and RTSP should work, and field reports are welcome.

Not sure whether your recorder qualifies? Send the brand, model, and firmware
through the [contact form](https://djxsystems.com/contact) for a pre-purchase
sanity check.

## What kinds of requests users make

Once recorders are registered by alias, requests are plain English. Typical
examples:

- "Pull the lobby camera from 8 to 9 PM yesterday and save it as an MP4."
- "Get me the loading-bay camera from 11:00 PM to 11:20 PM last night."
- "Download the parking-lot camera for the half hour starting 6:00 AM three days
  ago."

The skill also understands convenient time shortcuts ("yesterday," "N days
ago," a specific time-of-day window) so common "last night" requests are
low-friction. Cameras and recorders are referred to by the aliases you set up,
so nobody has to type IP addresses.

## What you get back

- **A verified MP4.** Each downloaded clip is checked with ffprobe for codec,
  resolution, frame rate, and duration. When the optional OCR tools (Pillow +
  tesseract) are present, the skill can also read the recorder's burned-in
  on-screen-display timestamp to confirm the clip really covers the time you
  asked for.
- **Browser-ready output.** Clips are written with faststart so they stream and
  seek instantly in a browser.
- **Joined multi-segment recordings.** When a recorder splits your window across
  several segments, the skill downloads each one and joins them into a single
  continuous file.
- **Tidy file organization.** Choose how clips are saved — flat, per-day,
  per-camera, or per-day-per-camera — set once during onboarding.

## How it works under the hood (at a high level)

- **Speaks the Dahua CGI protocol**, with an RTSP path available as a fallback.
- **Smart format handling.** It produces a playable MP4 with faststart
  regardless of the recorder's native format, so you get a clean, browser-ready
  file either way.
- **Strategy ladder with backoff and retry.** It tries the fast CGI download
  first, backs off and retries on throttling, and falls through to RTSP playback
  for stubborn firmware. On firmware that returns short downloads, it recovers
  the full requested window so you do not end up with a truncated clip. Every
  step is a structured progress event.
- **Built for AI agents.** It emits NDJSON progress events so your agent can run
  a download as a background task and keep the conversation responsive while a
  long export runs.
- **Guided onboarding.** Setup opens a real terminal for secure credential entry
  and can install ffmpeg for you with your confirmation (via winget, Homebrew,
  or apt), so you are not left hunting for prerequisites.

## Honest caveats

We would rather you know these up front than be surprised after buying:

- **Trimming is keyframe-approximate.** The start is accurate to within roughly
  one to three seconds, not the exact frame. This is ideal for "show me roughly
  8 to 9 PM" review work; it is **not** a frame-exact, evidence-grade export
  tool.
- **No audio.** Audio is dropped during conversion — Dahua-family recorder audio
  is inconsistent and rarely useful for review.
- **It does not replace the recorder's web UI.** It automates one specific pain
  point — finding and exporting recorded footage by camera and time.
  Administration, live view, and normal recorder management still happen in the
  web interface.

## AI agent compatibility

Built on the open **Agent Skills ([SKILL.md](https://agentskills.io)) standard**
— it works with any agent that can read a SKILL.md skill and run its scripts:
Claude Code & Desktop, OpenAI Codex, Cursor, Cline, Aider, Gemini CLI, and more.

**Verified hands-on with Claude and Codex.** The remaining agents work via the
open standard rather than having each been separately tested.

**Prerequisites:**

- Windows, macOS, or Linux with Python 3.10+.
- `ffmpeg` and `ffprobe` available on PATH (onboarding can install these for you
  with your confirmation).
- Network access from the agent machine to the recorder (HTTP/HTTPS for CGI,
  plus RTSP for the fallback path).
- A recorder account with permission to search and play back recorded footage.

## Security basics

- **Credentials are stored in your OS keyring** when possible (Windows
  Credential Manager / macOS Keychain / Linux Secret Service), or in a local
  `.env` file for headless setups (restricted to your user account on macOS and
  Linux). Credentials never appear on command lines or in logs.
- **Self-signed certificates.** Recorders on a LAN commonly use self-signed
  certificates, which the skill accepts for local connections. Keep your
  recorder on a **local network or VPN** rather than exposing it to the public
  internet.
- **Account-lockout aware.** The skill uses a single authentication attempt per
  method rather than retrying in a loop, to avoid tripping recorder account
  lockouts.
- **No telemetry, no auto-update, never executes downloaded code.** It contacts
  only the recorders you register and runs ffmpeg/ffprobe locally.

## Pricing and license

One-time purchase: **$29** (regularly $39). Or get **both NVR skills for $50**.
Licensed under the
[PolyForm Internal Use License 1.0.0](https://polyformproject.org/licenses/internal-use/1.0.0)
— this is a paid product, not open source.

## Where to buy

- **Dahua product page:** https://djxsystems.com/skills/dahua-nvr
- **Skills catalog (and the $50 bundle):** https://djxsystems.com/skills
- **Also listed on Agensi:** https://www.agensi.io/skills/dahua-ic-realtime-nvr

## Learn more

- **Run Hikvision cameras too?** See the [Hikvision NVR skill](./hikvision-nvr.md)
  — same idea, built for Hikvision's ISAPI protocol. Running a mixed fleet of
  both Dahua-family and Hikvision recorders? The two skills are sold separately
  and pair cleanly.
- **Repo overview:** [../README.md](../README.md)

---

*Independent and unaffiliated. This is a standalone skill from DJX Systems for
working with Dahua-protocol recorders. Dahua, IC Realtime, and the other brand
names are trademarks of their respective owners.*
