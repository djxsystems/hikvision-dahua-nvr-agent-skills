# Example Prompts

Real, plain-English requests you can give your AI agent once one of the DJX
Systems NVR skills is installed and your recorders are registered. The whole
point of these skills is that **the prompt is the interface** — you describe the
camera and the time window in ordinary language, and the agent handles the
protocol, the download, the conversion, and the verification.

These are *user prompts only*. They show how to talk to the agent; they do not
reveal how the skill works internally.

- **Hikvision skill details:** [docs/hikvision-nvr.md](./hikvision-nvr.md)
- **Dahua / IC Realtime skill details:** [docs/dahua-ic-realtime-nvr.md](./dahua-ic-realtime-nvr.md)
- **Compatibility:** [docs/compatibility.md](./compatibility.md)
- **Back to repo overview:** [../README.md](../README.md)

---

## How to read these examples

Both skills work the same way from the user's seat:

1. **Register each recorder once** with a short alias (a nickname like `front`,
   `warehouse`, or `site-b`). After that you refer to the recorder by name
   instead of typing an IP address.
2. **Ask for a camera and a time window** in plain English.
3. The agent finds the footage, downloads it, converts it to a standard MP4,
   trims it to your window, verifies the result, and tells you where it saved
   the file.

Most of the prompts below work with **either** skill — the request language is
the same whether you run a Hikvision or a Dahua-family recorder. The
skill-specific sections at the end call out the few differences worth knowing.

A reminder on accuracy, so your prompts match what the skill actually delivers:

- **Trimming is keyframe-approximate** — the start lands within about one to
  three seconds of what you ask for, not the exact frame. Great for "show me
  roughly 8 to 9 PM" review work; not a frame-exact, evidence-grade export.
- **No audio** — audio is dropped during conversion.
- Asking for a window with **no recording** gets you a clear "nothing for that
  time" answer, not a fake failure or an empty file.

---

## 1. The everyday clip

The bread-and-butter request: one camera, one time window, save it as an MP4.

> Pull the front-door camera from 8 to 9 PM yesterday and save it as an MP4.

> Get me the lobby camera from 2:15 PM to 2:35 PM yesterday.

> Download the loading-bay camera between 11:00 PM and 11:20 PM last night.

> I need the parking-lot camera from 6:00 to 6:30 this morning.

> Grab the warehouse entrance from 14:00 to 14:45 today. 24-hour time is fine.

## 2. Time shortcuts

Both skills understand convenient shortcuts so the common "last night" requests
stay low-friction.

> Pull the back-gate camera from 10 to 11 PM the night before last.

> Get the reception camera for the 20-minute window starting 9:00 AM three days
> ago.

> Download the side-entrance camera from midnight to 1 AM, two days ago.

> Give me the dock camera for the half hour around 3:30 PM yesterday.

> Pull last night's footage from the stairwell camera, 1 AM to 1:30 AM.

Tip: if you give a single time instead of a range ("around 2:30 yesterday"), the
agent will usually ask how long a window you want, or pick a sensible short one.
Being explicit about the range — start and end — gets you the cleanest result.

## 3. Naming the output and where it goes

You can describe the filename or folder you want; the agent will follow your file
organization setup (flat, per-day, per-camera, or per-day-per-camera) chosen
during onboarding.

> Pull the front-door camera from 8 to 9 PM yesterday and name the file
> `frontdoor-incident-review.mp4`.

> Download the lobby camera from 2 to 2:30 PM today and save it in the
> `insurance-claim` folder.

> Get the warehouse camera from 11 PM to midnight last night — call it
> `warehouse_2026-06-27_2300.mp4`.

## 4. Multi-camera requests

Ask for several cameras in one go. The agent works through them and can run the
downloads as background tasks so the conversation stays responsive.

> Pull the front-door, lobby, and parking-lot cameras from 8 to 9 PM yesterday.

> Get me every camera on the `warehouse` recorder for the five minutes around
> 2:31 PM today.

> Download the back-gate and side-entrance cameras from 11:00 to 11:30 PM last
> night, each as its own MP4.

> I need the loading-bay and dock cameras from 6 to 6:15 this morning — save them
> in a folder called `morning-delivery`.

## 5. Multiple recorders / sites

Because recorders are registered by alias, a single prompt can target a specific
site without anyone memorizing IP addresses.

> Pull the entrance camera on `site-b` from 7 to 7:30 AM today.

> Get the lobby camera from the `downtown` recorder and the lobby camera from the
> `airport` recorder, both from 9 to 9:15 AM yesterday.

> Which recorders do I have registered?

> Download the dock camera on `warehouse-2` from 3 to 3:20 PM yesterday.

## 6. Registering a recorder by alias

First-time setup, or adding another recorder later. The agent walks you through
secure credential entry; you never paste a password into the chat.

> Register a new recorder. I'll call it `front`. Walk me through entering the
> credentials securely.

> Add my warehouse NVR as `warehouse` and use the secure credential prompt — I do
> not want the password in the chat log.

> Set up a recorder called `site-b` for me, then test it by listing what cameras
> it has.

> I need to add a second Dahua recorder. Let's nickname it `loading-dock` and run
> the guided onboarding.

A few setup-adjacent prompts that are handy early on:

> Do I have ffmpeg installed? If not, install it for me — ask before you do.

> List the recorders I've already registered and which ones still need
> credentials.

> Update the saved credentials for the `front` recorder.

## 7. Background downloads and progress

Longer windows can take a while. These skills emit progress events so the agent
can run the download in the background and keep you posted.

> Pull the warehouse camera for the full hour from 1 to 2 AM last night — run it
> in the background and let me know when it's done.

> Start the loading-bay download from 11 PM to midnight and summarize progress as
> it goes; I'll keep working in the meantime.

> Kick off downloads for all three entrance cameras from 8 to 9 PM yesterday and
> ping me as each finishes.

## 8. Verifying the result

The skill verifies every clip automatically (codec, resolution, frame rate,
duration; plus the on-screen-display timestamp when the optional OCR tools are
present). You can also just ask.

> After you download the lobby clip, confirm it's real video and tell me its
> length, resolution, and frame rate.

> Pull the front-door camera from 2 to 2:15 PM yesterday and check that the
> on-screen timestamp actually matches the window I asked for.

> Did that clip come back the right length? What did the verification report?

## 9. When something isn't there

Honest behavior matters more than a confident-sounding wrong answer. These
prompts surface the skill's clear, typed handling of edge cases.

> Pull the side-gate camera from 3 to 4 AM last night. If there's no recording
> for that window, just tell me — don't guess.

> Is there any footage on the dock camera between 2 and 3 PM yesterday?

> The export looked short. Was the full window available, or did the recorder
> only have part of it?

---

## Skill-specific notes

The request language above is shared. Here are the few differences worth knowing
per skill.

### Hikvision NVR skill

Channel IDs vary by model, and you never supply the channel or worry about
segment boundaries — the skill keeps channel maps out of your prompt. Your
prompts stay exactly as simple as the examples above.

> Pull the front-door camera from 8 to 9 PM last night. (The skill figures out
> the right channel — you don't supply it.)

If a direct download is throttled or fails, the skill backs off, retries, and can
fall through to an RTSP playback path automatically. You can mention the priority
if you like:

> Get the lobby camera from 2 to 2:30 PM today. Prefer the fast download, but
> fall back if you have to — I just want the clip.

### Dahua / IC Realtime NVR skill

Works across the Dahua-protocol family — Dahua, IC Realtime, newer Lorex,
Amcrest, Honeywell Performance, the EZVIZ business line, and similar OEM
rebrands. (Older Lorex models use a different protocol and are not supported.)
Format differences between recorders are handled for you, so the prompt never
changes.

> Pull the lobby camera from 8 to 9 PM yesterday off my IC Realtime recorder.

If a fast download comes back short, the skill recovers the rest automatically —
you do not have to ask, but you can confirm:

> Download the warehouse camera from 11 PM to midnight last night. If the file
> looks short, recover the rest of the window so I get the full clip.

---

## A note on writing good prompts

You do not need special syntax — these skills are built to read ordinary
language. A few habits make results more reliable:

- **Give a start and an end time.** A clear window ("8 to 9 PM") beats a single
  moment ("around 8").
- **Say the date plainly.** "Yesterday," "last night," "three days ago," or an
  explicit date all work.
- **Name the camera and recorder by the aliases you set up.** That is what keeps
  the prompt short and avoids channel-number guessing.
- **Ask for what you want back.** A filename, a folder, "run it in the
  background," or "verify the timestamp" are all fair game.

For anything the skill cannot do — frame-exact trims, audio, or treating output
as evidence-grade — see the honest caveats in each skill's page linked at the top
of this document.
