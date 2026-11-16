# TabTape - Tab Audio Recorder

[中文](languages/README_zh.md)

Record any browser tab's audio to MP3 or WAV. Built for lectures, webinars, podcasts and meetings that live in a tab — with the pain points of existing recorders fixed:

- **No length cap** — audio is encoded and written to disk in ~1s chunks while recording, so memory stays flat. Free: 20 minutes per recording. Pro: unlimited.
- **Small files** — MP3 at 128 kbps (standard) or 64 kbps (half the size, great for speech); WAV when you need lossless.
- **Never lose a recording** — stopping saves into the built-in library (IndexedDB) and auto-downloads the file. Closing the tab cannot eat your audio.
- **Keep hearing the tab** — capture re-routes audio to your speakers instead of muting it.
- **Library** — play, rename, download, delete; shows storage used.
- **Pro trim** — pick a start/end, preview the selection, export it (and optionally keep it as a new recording). WAV cuts are byte-exact; MP3 cuts are frame-granular. No full-file decode, so long files stay safe.
- **Minimal permissions** — `storage`, `tabCapture`, `offscreen`, `downloads`, `alarms`, `unlimitedStorage`, `activeTab`. No `<all_urls>`, no content scripts, no account, no cloud. Recordings never leave your machine.

## Free vs Pro

| | Free | Pro |
|---|---|---|
| Record tab audio (MP3/WAV) | ✅ | ✅ |
| Length per recording | 20 min | Unlimited |
| Library + auto-download | ✅ | ✅ |
| Trim & export selection | — | ✅ |

One license key activates two browsers; removing it frees the slot. Validation is throttled to ~every 2 hours and works offline from cache; a failed validation never locks features.

## How it works

1. Open the tab you want to record, click the TabTape icon, press **Record this tab** (or Alt+Shift+R).
2. The tab keeps playing audibly while a red REC badge shows.
3. Press **Stop** (or the same shortcut). The file lands in your library and Downloads/TabTape/.
4. Open the library to play, rename, download, delete — or trim with Pro.

## Privacy

- Everything is local: IndexedDB for recordings, `chrome.storage.local` for settings and license state. No analytics, no telemetry.
- The only network requests are license activation/validation against `api.annmax1983.com`.
- MP3 encoding uses a vendored copy of [lamejs](https://github.com/zhuker/lamejs) (LGPL, see `vendor/lamejs-LICENSE.txt`; unminified source kept in `vendor/lame.js`).

## Permissions

| Permission | Why |
|---|---|
| `tabCapture` | Capture the current tab's audio (only after you click) |
| `offscreen` | MV3 requires an offscreen document to hold the media stream |
| `downloads` | Auto-save the finished file |
| `alarms` | Stop a recording that hits the free time limit, even if the worker is asleep |
| `unlimitedStorage` | Keep long recordings in the local library without hitting the storage quota |
| `storage` | Settings + license state |
| `activeTab` | Read the current tab's title/URL when you invoke the extension |

## Supported browsers

Chrome and Edge (Manifest V3, Chrome 116+). Firefox is not supported: it has no `tabCapture` equivalent.

## Development

```bash
npm test        # node --test: pure logic, MP3 encoder smoke, background mocks, i18n coverage
npm run build   # terser + zip → publish/tabtape-v<version>.zip
npm run icons   # regenerate icons/*.png (rounded-tile artwork, no dev dependency)
```

`npm run build` minifies every runtime script (the AudioWorklet processor included), copies `vendor/lame.min.js` byte for byte, and leaves the unminified `vendor/lame.js` out of the package. A minify failure or a reference to a file that is not in the package aborts the build without writing a zip.

Load the repository root as an unpacked extension (`chrome://extensions` → developer mode) to try it.

## Links

- Site: https://annmax1983.com
- License & pricing: https://annmax1983.com/checkout?plugin=tabtape