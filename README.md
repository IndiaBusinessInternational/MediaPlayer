# IBI Media Player v1.2.1

**One voice at a time; a host can refresh the playlist (v1.2.1)** — 29 Sep 2026, CEO in the Grower: "2 voices heard simultaneously … not playing properly". A page redraw had left a second, unseen player running; both answered Space, so one played hidden while the visible one showed paused. Now (1) starting any player pauses every other player on the page, the standard of every media app; (2) a player no longer on the page ignores the keyboard; (3) `destroy()` really stops: it pauses, unloads the file, cancels read-aloud and removes every page-level listener (one AbortController). New `api.sync(list)` replaces the playlist with a host's fresh list in the host's order without interrupting playback. The playing item stays current, durations already read and the user's own opened files are kept, and the list keeps its scroll position.


**Resizable playlist + the player no longer repaints its host (v1.2.0)** — 28 Sep 2026, CEO: "the line separating [the player and the playlist] to be moved flexibly — to the left or to the right, such that we can see clearly in PlayList the date of the video", and "when we click the Player section, the bg becomes white … every section … until we refresh". (1) On a desktop-width screen (≥ 900 px) the line between player and playlist is a divider you drag left or right, like the playlist pane of Windows Media Player or the sidebar of VS Code: 240 px up to 70 % of the player, remembered (`ibp:listW`), arrow keys move it 20 px (Shift 60), Home / End = narrowest / widest, double-click or Enter = the default 340 px. On a phone the playlist sits below the player, so there is no divider. (2) `styles.css` used to carry the standalone page's own `:root` colours, its light theme (which follows Windows' light mode), `body` and `*` rules — loaded inside the Grower they turned the whole Grower white. They now live in `page.css`, loaded only by the standalone page; `styles.css` styles the player component alone and reads the host's `--card`/`--txt`/… with dark fallbacks. (3) The standalone header's theme button is now a real toggle switch — a track and knob labelled **Dark** / **Light** (`role="switch"`), still following the device until you flip it.


**Light-theme buttons (v1.1.2)** — 27 Sep 2026: inside the Read-aloud and Speed menus and the playlist header, the secondary buttons (Pause, Stop, Open .txt, Set as default, Open, Folder, Captions, Clear) were white on white in the light theme; they now take the theme's text and card colours.

**Default voice + gender labels (v1.1.1)** — 27 Sep 2026: Google's "US English" (female, online) is the built-in default voice wherever the device has it (the CEO's choice; a saved Set-as-default still wins), and a voice's gender is read from the word in its name first ("UK English Female" was labelled Male).

**Read text aloud, in any voice on the device (v1.1.0)** — 27 Sep 2026, CEO: "provide as many voices as possible both female and male voices so that we can select, also set a default voice". A 🗣 button opens a panel: paste text or open a .txt, choose a voice from every voice the browser offers (grouped India / English / other, gender shown where the name tells), speed 0.5×–3×, Read / Pause / Stop, and Set as default (remembered as `ibp:voice`). Reading pauses the media; playing media stops the reading.

**Install button (v1.0.1)** — 27 Sep 2026: an Install button in the header appears whenever Chrome can install the app right there. Opened from another installed app, the page lands in a Chrome Custom Tab, which can never install; the fix is ⋮ → Open in Chrome first.

**A media player for the browser, built to the standard of YouTube Premium, VLC and Windows Media Player (v1.0.0)** — 27 Sep 2026, CEO: "a new web app similar to Windows Media Player that plays audio and video files of all the current versions or models … audio to run at speeds as per what YouTube Premium has … in between speeds … also the manual movement using cursor … under IBI YouTube Grower, also as a separate web app."

Live: https://player.indiabusinessinternational.online/ (GitHub Pages, public repo `MediaPlayer`). Also mounted inside IBI YouTube Grower as the 🎧 Player page, from the same `app.js`.

## What it does
- **Formats:** everything the browser can decode — MP4 (H.264, and H.265 where the device has the codec), WebM (VP9, AV1), MKV and MOV (Chrome, when the codec is installed), MP3, M4A/AAC, WAV, FLAC, OGG/Opus, WebA. A file the browser cannot decode gets a plain message with the fix (convert to MP4 or MP3). No server: files never leave the device.
- **Speed:** 0.25× to 4× in 0.05 steps. Presets 0.25 · 0.5 · 0.75 · Normal · 1.25 · 1.5 · 1.75 · 2, plus a custom slider and −/+ 0.05 buttons. The voice's pitch is preserved at every speed (switchable). Keyboard `<` `>` step 0.25, `Alt+,` `Alt+.` step 0.05.
- **Seeking:** drag or tap the seek bar (pointer events, so finger and mouse behave the same), hover time, buffered range, ±10 s buttons, `←` `→` 5 s, `0`–`9` to a tenth, `,` `.` frame step while paused.
- **Playlist:** open files or a whole folder, drag-and-drop files or folders anywhere, re-order by drag, remove, clear, previous/next, shuffle, repeat off/all/one. Durations fill in as files are read.
- **Captions:** `.vtt` or `.srt` (converted on the fly) on any video; `C` toggles.
- **Picture-in-picture**, **full screen** (the whole player, so the controls stay), controls auto-hide while video plays.
- **Remembers** the position in each file, the volume, mute, speed, repeat mode and theme.
- **Lock-screen and headset controls** through the Media Session API.
- **Installable** (PWA kit: manifest, icons, network-first service worker) and registered as a handler for audio/video files, so "Open with → IBI Media Player" works once installed. `?src=<url>&title=<name>` opens a file directly.
- **Phone-first:** audited at 360 px, 44 px targets, 16 px body, `100dvh` layout, dark by default and light when the device asks.

## Keyboard
Space/K play · J/L ±10 s · ←/→ ±5 s · ↑/↓ volume · M mute · F full screen · C captions · < > speed ±0.25 · Alt+, Alt+. speed ±0.05 · , . frame step (paused) · 0–9 seek · Home/End · N/P next/previous · I picture-in-picture · R repeat · Esc close.

## Embedding
```html
<link rel="stylesheet" href="styles.css"><script src="app.js"></script>
<div id="p"></div>
<script>IBIPlayer.mount(document.getElementById("p"), { items: [{ url: "/media/a.mp4", title: "A", poster: "/media/a.jpg" }], showOpen: true, onTitle: (t) => {} });</script>
```
The player takes the height of its container; colours follow `--accent`, `--card`, `--txt` when the host defines them.

## Versioning
Badge in `index.html` + this heading + `?v=` on the two asset tags + annotated `vX.Y.Z` tag + `sw.js` CACHE name, all together.
