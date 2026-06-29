# Sample outputs

These files are **illustrative shapes, not real traces.** They show the *kind* of
output the paid Hikvision and Dahua / IC Realtime NVR skills produce, so you can see
how an AI agent would consume the results before you buy.

> **Read this first**
>
> - These samples are **not runnable** and are **not the skill's real schema.** They are
>   hand-written examples with obviously fictional placeholder values.
> - Every value here is made up: camera aliases, timestamps, durations, file names, and
>   identifiers. No real recorder, IP address, hostname, credential, or protocol detail
>   appears anywhere in this repository.
> - Field names and nesting are kept **generic and high-level** on purpose. They are meant
>   to communicate structure (what an event or manifest *looks like*), not to document the
>   exact contract the paid skill emits. The real output may differ in field names, order,
>   and detail.
> - Nothing here reveals how the skills locate footage, choose a download strategy,
>   authenticate, or handle recorder-specific formats. That logic lives only in
>   the paid packages. See the repository [README](../README.md#what-this-repo-does-not-include)
>   for what this repo deliberately omits.

## Files in this folder

| File | What it illustrates |
| --- | --- |
| [`progress-events.ndjson`](progress-events.ndjson) | The **shape** of the newline-delimited JSON progress stream an agent reads while a download runs as a background task. Generic stage names (searching → downloading → converting → verifying → done) with placeholder values. |
| [`example-output-manifest.json`](example-output-manifest.json) | The **shape** of the result manifest written after a successful export — output file name, resolution, duration, codec, and a `verified` flag. Fictional values only. |
| [`redacted-config.example.json`](redacted-config.example.json) | An **illustrative** config template with placeholder keys and `REDACTED` / `your-alias-here` style values. Not the skill's actual config schema, and it does not reveal how credentials are handled. |

## How an agent uses these (at a glance)

1. You ask in plain English: *"pull the lobby camera from 8 to 9 PM yesterday."*
2. The skill dispatches the work as a background task and streams progress events
   (see `progress-events.ndjson`). Your agent can summarize status while the download runs.
3. On success, a result manifest (see `example-output-manifest.json`) describes the MP4 that
   was written, including the `ffprobe`-checked properties and a `verified` flag.
4. Recorder aliases and output preferences are set once during onboarding; a redacted,
   illustrative example of that kind of configuration is in `redacted-config.example.json`.

## Honest reminders that apply to real output too

- **Trimming is keyframe-approximate** — the start is accurate to within about one to three
  seconds, not frame-exact. This is review-grade, not an evidence-grade export tool.
- **Audio is not included** — it is dropped during conversion.
- Real credentials are stored in your OS keyring or a local `.env` file (restricted to your
  user on macOS/Linux) and **never** appear in any output, manifest, log, or command line —
  which is why nothing resembling a credential appears in these samples.

---

Want the real thing? See the [Hikvision](https://djxsystems.com/skills/hikvision-nvr) and
[Dahua / IC Realtime](https://djxsystems.com/skills/dahua-nvr) product pages, or the
[catalog](https://djxsystems.com/skills). Not sure your recorder is a fit? Send the brand,
model, and firmware through the [contact form](https://djxsystems.com/contact).
