# Hikvision NVR AI Agent Skill

Pull recorded footage off a Hikvision DVR or NVR by asking your AI agent in
plain English — no web UI, no channel-number guessing, no hand-built ffmpeg
commands.

> "Pull the front-door camera from 2:15 PM to 2:35 PM yesterday and save it as
> an MP4."

That sentence is the whole interface. The skill finds the recording, downloads
it, converts it to a clean browser-ready MP4, trims it to your window, and
verifies the result before reporting success.

- **Buy / catalog:** https://djxsystems.com/skills
- **Hikvision product page:** https://djxsystems.com/skills/hikvision-nvr
- **Also listed on Agensi:** https://www.agensi.io/skills/hikvision-nvr
- **Run Dahua / IC Realtime recorders too?** See the companion
  [Dahua / IC Realtime NVR skill](./dahua-ic-realtime-nvr.md).
- **Back to repo overview:** [../README.md](../README.md)

---

## The pain it removes

Getting last night's footage off a Hikvision recorder usually means logging
into the web interface, guessing which channel maps to which camera, wrestling
with ISAPI XML or copying an RTSP playback URL into ffmpeg by hand, then
squinting at the result to check whether it actually covers the time you asked
for. It is fiddly, easy to get wrong, and it does not scale when somebody asks
for "the loading dock around 2:30" once a week.

This skill hands that entire chore to an AI agent. You register each recorder
once with a short alias, and from then on a request is a sentence. The agent
locates the footage, picks the best download strategy, clips to the requested
window, and confirms the output is real video of plausible length — while
reporting progress in a format another agent can summarize as the download
runs.

## Supported recorders

Built around **Hikvision DVRs and NVRs and compatible OEM rebrands that expose
the ISAPI interface** (with RTSP available as a fallback path). If your
recorder speaks Hikvision's ISAPI, it is very likely a fit, including
OEM-rebranded units sold under other names.

**Validated hands-on across four recorder models spanning firmware in the V3.x
through V4.5 range**, covering older DVR firmware, newer NI-family NVRs, and
mixed fleets where model families do not return the exact same XML shape. Other
Hikvision and OEM-rebrand recorders that speak ISAPI should work too.

Not sure whether your recorder qualifies? Send the model number and firmware
version through the [contact form](https://djxsystems.com/contact) for a
pre-purchase sanity check.

## What kinds of requests users make

Once recorders are registered by alias, requests are plain English. Typical
examples:

- "Pull the front-door camera from 8 to 9 PM last night and save it as an MP4."
- "Get me the loading-bay camera from 2:15 to 2:35 PM yesterday."
- "Download the lobby camera for the 20-minute window starting 11:00 PM two
  days ago."

The skill also understands convenient time shortcuts ("yesterday," "N days
ago," a specific time-of-day window) so common "last night" requests are
low-friction. It refers to cameras and recorders by the aliases you set up, so
nobody has to memorize channel-to-camera maps or type IP addresses.

## What you get back

- **A verified MP4.** Each downloaded clip is checked with ffprobe for codec,
  resolution, duration, and frame rate. When the optional OCR tools (Pillow +
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

- **Speaks Hikvision ISAPI**, with an RTSP playback path as a fallback and an
  understanding of the protocol's real-world quirks.
- **Strategy ladder with backoff and retry.** It tries the fast direct download
  first, backs off and retries on throttling, and falls through to RTSP
  playback only when needed. Every transition is announced as a structured
  progress event.
- **Reliable across recorder quirks.** Recorders vary in how they report
  recording boundaries; the skill handles those differences so you get the
  footage you actually asked for.
- **Built for AI agents.** It emits NDJSON progress events so your agent can
  dispatch a download as a background task, stream summarized status, and stay
  responsive while a long export runs.
- **Guided onboarding.** Setup opens a real terminal for secure credential entry
  and can install ffmpeg for you with your confirmation (via winget, Homebrew,
  or apt), so you are not left hunting for prerequisites.

## Honest caveats

We would rather you know these up front than be surprised after buying:

- **Trimming is keyframe-approximate.** The start is accurate to within roughly
  one to three seconds, not the exact frame. This is ideal for "show me roughly
  8 to 9 PM" review work; it is **not** a frame-exact, evidence-grade export
  tool.
- **No audio.** Audio is dropped during conversion.
- **It does not replace the recorder's web UI.** It automates one specific pain
  point — finding and exporting recorded footage by time range. Administration,
  live view, and normal recorder management still happen in the web interface.

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
- Network access from the agent machine to the recorder.
- A recorder account with permission to search and play back recorded footage.
- RTSP enabled on the recorder for the fallback path.

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

- **Hikvision product page:** https://djxsystems.com/skills/hikvision-nvr
- **Skills catalog (and the $50 bundle):** https://djxsystems.com/skills
- **Agensi listing:** https://www.agensi.io/skills/hikvision-nvr

## Learn more

- **Run a different brand of recorder?** If your cameras run on Dahua or a
  Dahua-protocol OEM (IC Realtime, newer Lorex, Amcrest, Honeywell Performance,
  EZVIZ business line), use the
  [Dahua / IC Realtime NVR skill](./dahua-ic-realtime-nvr.md) instead. Running a
  mixed fleet of both? The two skills are sold separately and pair cleanly.
- **The manual version first.** The technical guide
  [Download Hikvision NVR footage by time range with ISAPI, RTSP, and ffmpeg](https://djxsystems.com/insights/hikvision-nvr-time-range-download)
  explains the workflow this skill automates.
- **Repo overview:** [../README.md](../README.md)

---

*Independent and unaffiliated. This is a standalone skill from DJX Systems for
working with Hikvision-protocol recorders. Hikvision and any OEM brand names are
trademarks of their respective owners.*
