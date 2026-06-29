# Security and responsible use

How the DJX Systems NVR AI Agent Skills handle your recorder credentials, what
they do and do not send over the network, and the one rule that matters most:
only access recorders you own or are authorized to use.

This page describes posture and behavior. It deliberately contains no
implementation detail.

- **Compatibility and network requirements:** [docs/compatibility.md](./compatibility.md)
- **Back to repo overview:** [../README.md](../README.md)

---

## Credential storage

Recorder credentials are kept out of sight and out of logs.

- **OS keyring when possible.** During onboarding the skill stores your
  recorder credentials in your operating system's secure store — **Windows
  Credential Manager**, **macOS Keychain**, or **Linux Secret Service**.
- **Local `.env` fallback for headless setups.** When a keyring is not
  available (for example, an unattended server), credentials go into a local
  `.env` file instead. On **macOS and Linux that file is restricted to your
  user account** (permissions `0600`).
- **Never in command lines or logs.** Credentials never appear on a command
  line or in any log or progress output. Recorders are referenced by short
  aliases you assign during onboarding, so prompts, logs, and saved files use a
  name rather than an IP address, username, or password.

We do **not** claim the local `.env` file is encrypted at rest. It is a plain
file protected by your user account's file permissions on macOS and Linux; use
the OS keyring when you can.

## Self-signed certificates and network posture

Recorders on a LAN commonly present a **self-signed certificate** over HTTPS.
The skill **accepts that self-signed certificate** for local connections so the
workflow is not blocked by certificate-trust errors on equipment you control.

Because the skill accepts self-signed certificates, **keep your recorder on a
local network or VPN — not the public internet.** A recorder exposed directly to
the internet is a risk regardless of this skill; the skill is designed for a
trusted LAN/VPN environment and assumes you are operating it there.

## Account-lockout protection

Many NVRs lock an account after a handful of failed authentication attempts. To
avoid tripping that lockout, the skill uses a **single authentication attempt
per method** rather than retrying credentials in a loop. If authentication
fails, it reports the failure cleanly instead of hammering the recorder.

## What the skill does NOT do

- **No telemetry.** It contacts only the recorders you register. Nothing about
  your usage, footage, or environment is sent to DJX Systems or any third party.
- **No auto-update.** It does not phone home for updates or change itself behind
  your back. You run the version you bought until you choose to update it.
- **Never executes downloaded code.** It talks to your recorder and runs
  `ffmpeg`/`ffprobe` locally — it does not fetch and run code from the network.

## Responsible use

These skills are tools for **exporting footage from recorders you own or are
explicitly authorized to access.** Use them only on equipment you control or
have written permission to operate.

- Do not point the skills at recorders, cameras, or networks you do not own or
  have not been authorized to access.
- Follow your organization's policies and any applicable laws on surveillance,
  recording, retention, privacy, and evidence handling in your jurisdiction.
- **Trimming is keyframe-approximate** — the start is accurate to within about
  one to three seconds, not frame-exact. The output is review-grade. It is **not
  an evidence-grade export tool**; do not represent a clip as a frame-exact or
  forensically precise record.
- **Audio is not included** — it is dropped during conversion. A clip is video
  only.

If you have a security question or want to confirm a deployment fits this
posture, reach us through the [contact form](https://djxsystems.com/contact).
