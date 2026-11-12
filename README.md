# Streamwisp

### Twitch broadcasting, watching and chat in Steam Gaming Mode

**Made by Danker__TV** · Version **1.0.0 Public** · Build **2026.11.12.2**

## AI development disclosure

Streamwisp has been developed with substantial assistance from AI tools, including code generation, debugging, interface design and documentation. Danker__TV directs the project and its feature decisions.

This is disclosed openly so you can make an informed choice about using or contributing to the project. AI assistance does not guarantee correctness, security or reliability. Features require testing, and the known limitations below apply. Bug reports, code reviews and constructive feedback are welcome.

## Overview

Streamwisp is a Steam Deck-focused [Decky Loader](https://github.com/SteamDeckHomebrew/decky-loader) plugin that brings Twitch broadcasting, local recording, stream watching and chat into Steam Gaming Mode. Manage your broadcast, switch to a custom BRB screen, follow another channel's chat and watch streams alongside your game from one controller-friendly workspace.

The interface includes a compact Steam panel and an expanded studio, organised into **Live Studio**, **Watch**, **Chat & Overlay** and **Settings**.

> **Development status:** Streamwisp is actively developed, with ongoing improvements to playback, performance and usability. Features and reliability can vary by device, Linux distribution and Gaming Mode session. Hardware acceleration, capture, Steam menu protection and browser-assisted raids require testing on your setup. Bug reports and feedback help guide future updates.

Based on experience so far, Streamwisp seems to work best on **Arch-based Linux distributions**, including SteamOS. This is an observation from testing rather than a guarantee of compatibility with every Arch-based system.

**Systems reported to work:** SteamOS and CachyOS. Results can vary with hardware, graphics drivers and the active desktop or Gaming Mode session.

## Features

### Live Studio: broadcast and record

- Start and stop Twitch broadcasts from Gaming Mode.
- Record locally without going live, or enable local recording alongside a broadcast.
- Select a recording destination, including detected removable storage, with available-space information.
- Edit your Twitch stream title and game category.
- Start with **Just Chatting** as the Twitch category when the plugin loads with a connected account, or after first-time sign-in. Your title is retained, and manual choices or the game detector can change the category afterwards.
- Automatically detect a running Steam game and attempt to match its Twitch category. Your existing category is retained if an exact match is unavailable.
- Manually detect the current game or use its name as your stream title.
- Create Twitch clips from an active broadcast, subject to account permissions and Twitch availability.
- Include microphone audio and mute or unmute it from the live controls.
- Configure audio bitrate and use the optional Steamcord audio integration when that integration is available.
- View capture, encoder and connection diagnostics. Local video output and Twitch's reported live state are treated separately.
- Repair the Twitch connection by refreshing the linked account's stream key and restoring the default destination, then start a new broadcast attempt.
- Configure the reconnection window: off, 15, 30, 60 or 120 seconds.

#### Broadcast quality and hardware

Choose **720p**, **800p**, **900p**, **1080p** or the captured source resolution, with **30 FPS** or **60 FPS** output options.

Each resolution displays video bitrate guidance for the selected frame rate. These are the values currently shown in Streamwisp:

| Output | Resolution | 30 FPS guidance | 60 FPS guidance |
| --- | --- | ---: | ---: |
| 720p | 1280 × 720 | 3,000 kbps | 4,500 kbps |
| 800p | 1280 × 800 | ~3,500 kbps | ~5,000 kbps |
| 900p | 1600 × 900 | ~4,000 kbps | ~6,000 kbps |
| 1080p | 1920 × 1080 | 4,500 kbps | 6,000 kbps |

`~` marks Streamwisp estimates for intermediate resolutions. Source-resolution guidance varies with the captured size. These values are guidance, not a guarantee of stream quality or an automatic bandwidth measurement.

Broadcast encoder choices include **Automatic**, **CPU software H.264**, **AMD/Intel VA-API** and **NVIDIA NVENC**, where supported by the local hardware checks. A recheck control and failure details help identify unavailable options. Software encoding also offers Light, Balanced and Quality effort presets.

Video bitrate, audio bitrate and keyframe interval are adjustable. Broadcast encoding and Watch playback decoding have separate settings. Apply output changes before starting the next broadcast or recording.

### BRB screens and privacy controls

- Switch to a bundled **“I'll Be Right Back”** art card during a session.
- Import a custom image or video through an in-plugin file browser.
- Browse the home directory, removable-storage locations or a manually entered folder.
- Restore the default artwork without deleting imported media.
- Custom videos loop without their own sound; still images retain their proportions.
- Supported import formats include PNG, JPG/JPEG, WEBP, GIF, MP4, WEBM, MKV and MOV, subject to successful decoding. The import limit is 1 GB per file.

The **Privacy shield** holds BRB video instead of screen capture. **Start behind privacy shield** lets new broadcasts and recordings begin with BRB output until you explicitly release it. Audio follows the microphone and audio controls.

**Freeze Steam side menus** is an experimental protection option that holds the picture when a Steam side menu opens or menu detection becomes unavailable. It uses an approximately one-second video/audio delay and bounded frame storage. Brief black capture frames after a menu closes can retain the last safe picture while capture recovers.

**Privacy limits:** These controls do not provide guaranteed game-only capture or reliable exclusion of every Steam dialog, notification or personal-information screen. Check the actual outgoing picture before opening sensitive content. Menu freezing can add latency and consume substantial RAM, particularly at high resolutions and 60 FPS.

### Watch Twitch while you play

- Watch up to **four Twitch streams** in the game overlay.
- Find channels through the followed-live list or enter channel usernames directly.
- View followed-channel names, thumbnails, game/title details and viewer counts.
- Adjust the overlay layout, size and opacity, or focus on one selected stream.
- Choose one channel for audio and adjust its volume independently of broadcast settings.
- Select Automatic frame rate, 30 FPS or Prefer 60 FPS, depending on available source renditions and device performance.
- Choose CPU playback or locally available AMD/Intel VA-API and NVIDIA decoding options where the playback checks allow them.
- Request H.264, HEVC/H.265 or AV1 when the channel provides a compatible rendition. These choices cannot convert a channel into a codec it does not offer.

#### Current player and adaptive quality

The current version uses a **bundled FFmpeg decoder and GTK/Cairo Watch overlay**. No separate player installation is required.

Watch selects source renditions up to **1080p**. When repeated or prolonged stalls are detected, it can switch to an available lighter rendition:

- Three detected stalls within a minute, or six seconds of sustained detected stalling, can trigger a step-down.
- Resolution is reduced before frame rate where possible.
- Quality changes briefly reconnect playback and are at least 30 seconds apart.
- Reduced quality is preserved across automatic reconnects until Watch is stopped and started again.

This depends on Twitch offering a suitable lower rendition. There is no guaranteed 60 FPS output, uninterrupted delivery or zero-delay live playback.

#### Resolution notifications and playback information

Steam notifications show the selected source resolution when playback starts and when quality changes, including automatic step-down. Notifications work while the Watch panel is closed and do not repeat for unchanged quality. Streamer mode suppresses these notices without queuing stale messages for later.

The playback information panel reports source resolution/FPS and the received and drawn frame rates. The displayed resolution refers to the selected Twitch rendition, not the size of the overlay tile.

### Chat and overlay

- Show Twitch chat over your game in a configurable overlay.
- Search for a channel and switch the selected chat destination.
- Let chat follow the selected Watch channel.
- Reset chat to your own channel after watching someone else.
- See the selected recipient in the message box before sending.
- Send messages using your connected Twitch account, subject to Twitch permissions and channel restrictions.
- Adjust overlay appearance, including text size, opacity, width, corner position and badge visibility.
- Receive visible startup or channel-switch errors rather than silently assuming the chat overlay is running.

### Offline speech-to-text

Streamwisp includes optional microphone-based speech recognition for two workflows:

| Workflow | Behaviour |
| --- | --- |
| Chat dictation — experimental | Record up to 60 seconds, review the recognised words and add them to a chat draft. Sending remains a separate action. |
| Live captions | Enable captions before starting a broadcast or recording. Captions are added to the outgoing picture and pause during microphone mute, BRB and privacy shield. |

Recognition runs locally with the bundled [Vosk engine](https://alphacephei.com/vosk/) and small English model. The interface targets **English (UK)** and applies UK spelling conversion. The included model is `vosk-model-small-en-us-0.15`; it is not a dedicated British-accent model.

**Dictation mode is experimental.** Recognition accuracy and microphone behaviour may vary by device and environment. Review and correct the draft before sending a message.

Speech recognition is off until enabled and uses the default microphone. Allow roughly **300 MB of additional RAM** while listening. Accuracy depends on accent, microphone quality and background noise.

### Twitch raids and ending a stream

- Choose a raid target from followed live channels or enter a username.
- Confirm the destination, view pending raid status and cancel a pending raid.
- Use manual Twitch handling or the optional **Streamwisp Raid Browser Helper** for automatic completion after Twitch's first countdown.
- Keep the broadcast running while a raid is pending. Automatic ending waits for Twitch confirmation of the selected source/target raid.

The optional helper supports Chromium-based browsers such as Chrome, Chromium, Brave and Edge. It is installed separately as an unpacked extension and must run **on the same device** as Streamwisp, with your broadcaster account logged in and your own Twitch popout chat open.

For the browser installation step, see Google's guide to [loading an unpacked Chrome extension](https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world#load-unpacked). Use the Streamwisp helper folder supplied with the package; you do not need to build the example extension in that guide.

Pairing is saved across normal restarts. Re-pairing may be required after signing out, disabling automatic raids, resetting the helper or a port conflict. The helper does not receive the Twitch OAuth access token.

For automatic raids, the helper attempts **Raid Now after the first 10-second countdown**, once Twitch enables the matching button. A click alone does not confirm viewer transfer: Streamwisp waits for Twitch's raid event before automatically ending the original broadcast. Missing confirmation leaves the broadcast running.

Browser suspension, changed Twitch page markup and unavailable session support can prevent automation. Firefox is not included in this helper build. A browser on a phone or another PC cannot pair with the loopback-only helper.

### RAM monitoring and power profiles

A shared **radio-tuner-style RAM gauge** is visible across the workspace:

- System usage shown as a percentage and used/total gigabytes.
- Numbered markers at 10% intervals, with smaller intermediate ticks.
- Green for usage up to 30%, amber above 30% through 70%, and red above 70%.
- An estimated memory figure for Streamwisp and its helper processes, already included in the system total.
- Updates approximately every five seconds.

Where the host provides a compatible system power-profile service, choose **Power Saver**, **Balanced** or **Performance**. Only profiles exposed by the host are available. This changes the system profile; it does not directly control Steam's wattage limit or automatically raise power when buffering occurs.

### Five themes and a controller-friendly workspace

| Theme | Appearance |
| --- | --- |
| Midnight | Charcoal with sea-glass teal accents |
| After Hours | Ink backgrounds with soft violet accents |
| Ember | Warm graphite and amber |
| Halloween | Pumpkin-orange highlights and burnt-orange surfaces |
| Christmas | Deep red surfaces with warm gold highlights |

The compact panel and expanded studio share the saved theme. Wisp branding follows the appearance system, with Christmas artwork featuring a Santa hat. The interface includes controller-focus outlines, grouped controls and reduced-motion styling.

**Settings → App & build** shows the installed version, build number and **Made by Danker__TV** credit.

### Dependency checks and setup assistance

The System setup page checks the environment needed by Streamwisp, including relevant Python/graphics bindings, FFmpeg capabilities, audio services, capture/display access and Gamescope session availability.

- Reports dependency readiness separately from session or hardware capability issues.
- Attempts supported dependency installation on recognised mutable Linux distributions when sufficient permissions are available.
- Rechecks after installation and reports progress and failures.
- Supplies host-specific guidance when installation is unsupported or cannot finish.
- Offers a browser setup guide with commands and diagnostic information where applicable.
- Saves timestamped text and offline HTML instructions in the user's Documents folder when setup cannot complete, with paths shown in the interface.

The Steam Deck package includes a private runtime containing many required userspace components. This reduces reliance on host packages but does not replace the host kernel, GPU drivers, desktop/session services, permissions or capture support.

SteamOS and other image-based systems require different handling from ordinary mutable distributions. Streamwisp does not promise automatic dependency installation or full compatibility on every Linux distribution.

## Installation

1. Install [Decky Loader using the official installation instructions](https://github.com/SteamDeckHomebrew/decky-loader#-installation) on your Steam Deck or supported Linux gaming setup.
2. Download the Streamwisp **1.0.0 Public** plugin ZIP supplied with the release.
3. Install it through Decky's plugin ZIP installer.
4. Restart Steam after replacing an existing build.
5. Open Streamwisp and connect Twitch under **Settings → Twitch account**.
6. Review **System setup** and check the required session/capture capabilities before broadcasting.

Disable or remove older copies of the plugin so multiple instances do not control the same stream or settings. Existing account settings, theme preferences and BRB selections retain their previous storage locations for compatibility.

Install and pair the browser helper separately only if you want browser-assisted raid automation. Updates to this custom Streamwisp build are installed manually from ZIP packages.

## Compatibility and current limitations

- **Primary target:** Steam Deck running SteamOS on x86_64 Linux, with Decky Loader and an accessible supported graphics/capture session.
- **Also reported working:** CachyOS, an Arch-based Linux distribution. Feature availability still depends on the device and session.
- Other Linux distributions may work, but display access, Gamescope, audio, GPU drivers and dependency packaging vary. Compatibility is not universal.
- Current Watch buffering fixes, capture, hardware acceleration, privacy timing and browser-assisted raids still require testing on real devices. Passing local or mocked checks does not establish end-to-end functionality.
- Twitch source quality, codecs, permissions and service behaviour affect available features.
- Menu privacy protection is a best-effort feature.
- The current build is Twitch-focused. YouTube playback, search and login are not included.
- Included userspace dependencies require disk space, including additional space for private runtime extraction on first use.

## Useful links

| Project or tool | Official website or documentation |
| --- | --- |
| Decky Loader | [Project and installation instructions](https://github.com/SteamDeckHomebrew/decky-loader) |
| Gamescope | [Valve's Gamescope project](https://github.com/ValveSoftware/gamescope) |
| FFmpeg | [Official website](https://ffmpeg.org/) |
| GTK | [Official website](https://www.gtk.org/) |
| PipeWire | [Official website](https://pipewire.org/) |
| Vosk | [Offline speech recognition](https://alphacephei.com/vosk/) and [available models](https://alphacephei.com/vosk/models) |
| Browser extension setup | [Chrome's unpacked-extension instructions](https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world#load-unpacked) |
| Twitch Inspector | [Broadcast connection and encoder diagnostics](https://inspector.twitch.tv/) |

These links provide project information and troubleshooting resources. The bundled Streamwisp package already supplies many userspace dependencies; this list is not a requirement to install each tool separately.

## Reporting problems

When reporting an issue, include the installed version/build, device and Linux distribution, whether you are in Gaming Mode or Desktop Mode, the affected panel and the exact error text. For playback problems, also include the source resolution/FPS, decoder choice and whether audio, video or both are affected.

Remove stream keys, access tokens and browser-helper pairing codes from screenshots and logs before sharing them.

## Credits and licensing

- **Streamwisp:** made by **Danker__TV**.
- **Offline speech recognition:** [Vosk by Alpha Cephei](https://alphacephei.com/vosk/).

Original BSD-3-Clause notices and attribution are retained in the supplied `LICENSE` and `NOTICE.txt`. Bundled third-party components retain their separate notices and terms. Review the licenses and attribution supplied with the package before redistributing it.
