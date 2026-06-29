# Compatibility

How the DJX Systems NVR AI Agent Skills fit into your environment: which AI
agents drive them, what your machine needs, which operating systems are
supported, and how the network has to be laid out. For the list of recorder
brands, models, and firmware, see the per-skill pages linked below.

- **Hikvision skill details:** [docs/hikvision-nvr.md](./hikvision-nvr.md)
- **Dahua / IC Realtime skill details:** [docs/dahua-ic-realtime-nvr.md](./dahua-ic-realtime-nvr.md)
- **Security and network posture:** [docs/security.md](./security.md)
- **Back to repo overview:** [../README.md](../README.md)

---

## The Agent Skills (`SKILL.md`) standard

Both skills are packaged as **Agent Skills** — the open
[`SKILL.md`](https://agentskills.io) standard. A skill is a folder containing a
`SKILL.md` file (instructions an agent reads) plus the scripts it runs. Any AI
agent that can read a `SKILL.md` skill and execute its scripts can use these
skills; there is no proprietary runtime, plugin, or account to wire up.

That openness is the point: you are not locked into a single vendor's agent. If
your agent understands the `SKILL.md` standard, it can drive the skill.

## AI agent compatibility

> Built on the open Agent Skills (`SKILL.md`) standard — works with any agent
> that can read a `SKILL.md` skill and run its scripts: Claude Code & Desktop,
> OpenAI Codex, Cursor, Cline, Aider, Gemini CLI, and more. Verified hands-on
> with Claude and Codex.

| Agent | Status |
| --- | --- |
| Claude (Code & Desktop) | Verified hands-on |
| OpenAI Codex | Verified hands-on |
| Cursor | Works via the open standard |
| Cline | Works via the open standard |
| Aider | Works via the open standard |
| Gemini CLI | Works via the open standard |
| Other `SKILL.md`-compatible agents | Works via the open standard |

**What "verified hands-on" means.** Claude and Codex were tested directly with
these skills end to end — that is a credibility signal, not a limit on
compatibility.

**What "works via the open standard" means.** The remaining agents are not
separately tested, but because the skills follow the `SKILL.md` standard, any
agent that reads a `SKILL.md` skill and runs its scripts should drive them. We
do not claim every agent in this category was tested individually.

If you run one of these skills in an agent we have not listed, we would like to
hear how it went — see [Report compatibility](#report-compatibility) below.

## Prerequisites

- **Python 3.10 or newer.**
- **`ffmpeg` and `ffprobe` available on your `PATH`.** These do the conversion,
  trimming, and verification. Guided onboarding can install them for you, with
  your confirmation (via winget, Homebrew, or apt), so you are not left hunting
  for prerequisites.
- **Optional:** Pillow and tesseract, if you want the on-screen-display
  timestamp OCR check. Everything else works without them.

## Operating systems

Supported on **Windows, macOS, and Linux**. The skills run wherever your agent
and Python run; credential storage adapts to each platform (see
[docs/security.md](./security.md)):

| OS | Credential store |
| --- | --- |
| Windows | Windows Credential Manager (or a local `.env` for headless use) |
| macOS | macOS Keychain (or a local `.env`, restricted to your user) |
| Linux | Linux Secret Service (or a local `.env`, restricted to your user) |

## Network requirements

The machine running the agent needs to **reach the recorder over the local
network**. There is no DJX-hosted service in the path — traffic goes straight
from your machine to your recorder.

- **HTTP / HTTPS** to the recorder for search and direct download.
- **RTSP** to the recorder for the playback fallback path. Keep RTSP enabled on
  the recorder so the fallback is available when a direct download is not.
- A **recorder account** with permission to search and play back recorded
  footage.

Recorders on a LAN commonly present a self-signed certificate over HTTPS, which
the skills accept for local connections. Because of that, keep your recorder on
a **local network or VPN** rather than exposing it to the public internet. See
[docs/security.md](./security.md) for the full reasoning.

## Report compatibility

Running a recorder, firmware version, or agent we have not listed? A short
report helps every other buyer.

- **Open an issue** using the templates under
  [.github/ISSUE_TEMPLATE/](../.github/ISSUE_TEMPLATE/). Please include the
  brand, model, firmware version, the agent you used, and what worked or failed.
- Prefer not to file publicly? Reach us through the
  [contact form](https://djxsystems.com/contact).

Not sure whether your recorder is a fit *before* buying? Send the brand, model,
and firmware through the [contact form](https://djxsystems.com/contact) for a
pre-purchase sanity check.
