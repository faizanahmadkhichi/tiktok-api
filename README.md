# Free TikTok API for builders

Drop any TikTok link in and get the video back as JSON — author, caption,
views, likes, comments, shares, plus direct download links for the video
(no watermark) and the audio. No login. No key. Free to use.

Built by [FAK LABS](https://faizankhichi.me) — [@faizanahmadkhichi](https://github.com/faizanahmadkhichi).

## How to use

One GET request. Put the TikTok URL in the `url` parameter:

```
https://tt-api.faizankhichi.me/?url=https://vt.tiktok.com/ZSpqnENeV/
```

Works with short links (`vt.tiktok.com`, `vm.tiktok.com`) and full links
(`tiktok.com/@user/video/123...`).

### cURL

```bash
curl "https://tt-api.faizankhichi.me/?url=https://vt.tiktok.com/ZSpqnENeV/"
```

### JavaScript

```js
const videoUrl = "https://vt.tiktok.com/ZSpqnENeV/";
const res = await fetch(`https://tt-api.faizankhichi.me/?url=${encodeURIComponent(videoUrl)}`);
const data = await res.json();
console.log(data.video.directUrl);         // click to watch, no cookies needed
console.log(data.video.directDownloadUrl); // forces a file download
```

### Python

```python
import requests

video_url = "https://vt.tiktok.com/ZSpqnENeV/"
data = requests.get(
    "https://tt-api.faizankhichi.me/",
    params={"url": video_url},
).json()

print(data["video"]["directUrl"])
print(data["music"]["directUrl"])
```

## What you get back

```json
{
  "success": true,
  "id": "7707295769895685396",
  "description": "In another life #lyrics #fypage #lyricsvideo #fypp",
  "canonicalUrl": "https://www.tiktok.com/@rrvenslyrics/video/7707295769895685396",
  "createTimeISO": "2026-11-16T...",
  "stats": {
    "plays": 0,
    "likes": 0,
    "comments": 0,
    "shares": 0,
    "saves": 0,
    "reposts": 0
  },
  "author": {
    "uniqueId": "rrvenslyrics",
    "nickname": "...",
    "verified": false,
    "profileUrl": "https://www.tiktok.com/@rrvenslyrics"
  },
  "video": {
    "duration": 21,
    "width": 0,
    "height": 0,
    "playUrl": "https://v16-webapp-prime.tiktok.com/...",
    "downloadUrl": "https://v16-webapp-prime.tiktok.com/...",
    "directUrl": "https://tt-api.faizankhichi.me/dl?url=...&cookie=...",
    "directDownloadUrl": "https://tt-api.faizankhichi.me/dl?url=...&cookie=...&download=1",
    "covers": {
      "default": "https://...",
      "origin": "https://..."
    },
    "bitrates": [
      {
        "qualityType": 0,
        "bitrate": 0,
        "width": 0,
        "height": 0,
        "urls": ["https://..."]
      }
    ]
  },
  "music": {
    "title": "...",
    "authorName": "...",
    "playUrl": "https://v45.tiktokcdn-eu.com/...",
    "directUrl": "https://tt-api.faizankhichi.me/dl?url=...&cookie=...",
    "directDownloadUrl": "https://tt-api.faizankhichi.me/dl?url=...&cookie=...&download=1"
  }
}
```

### Fields

| Field | What it is |
|---|---|
| `video.directUrl` | Click to watch — plays in the browser, no cookies needed |
| `video.directDownloadUrl` | Same video, forces a file download |
| `music.directUrl` / `music.directDownloadUrl` | Same pair for the audio track |
| `video.playUrl` / `downloadUrl` | Raw TikTok CDN links (need the session cookie — use the direct links instead) |
| `video.playUrls` | Every video source URL TikTok returned |
| `video.bitrates` | Quality variants with bitrate, size, and URLs |
| `video.covers` | Poster images (`default`, `origin`, `dynamic`, `zoom`) |
| `stats.plays` / `likes` / `comments` / `shares` / `saves` / `reposts` | Public counts |
| `author.uniqueId` / `nickname` / `verified` | Who posted it |
| `author.stats` | Follower, following, like, and video counts |
| `hashtags` | Tags on the video, with links |
| `success` | `true` when the video was found |

## Direct media links (`/dl`)

The `directUrl` links go through `/dl`, which serves the media with the
session attached server-side — the clicker needs no cookies or headers.

- Watch: open `video.directUrl` — plays inline in the browser, seeking works.
- Download: open `video.directDownloadUrl` — saves as `tiktok-video.mp4`
  (audio saves as `tiktok-audio.mp3`).

## Options

- `&raw=1` — include the full raw TikTok item in the response.

## Errors

- `400` — missing or invalid `url`, or not a TikTok link.
- `404` — video not found, private, or deleted.
- `502` — TikTok didn't return the video data, try again shortly.
- `403` — direct access only. Server-to-server proxying is not allowed.

## Rules

- Free to use, no key, no sign-up. Be fair: don't hammer it.
- If a video is deleted, private, or the link is wrong, `success` is `false`.
- The API itself is hosted by FAK LABS. This repo is documentation only —
  the implementation is not public.

## Built something with it?

Show it in the CODE LAB channel on WhatsApp
or open an issue here with a link.

## More from FAK LABS

- Website: https://faizankhichi.me
- GitHub: https://github.com/faizanahmadkhichi
- WhatsApp: https://wa.me/923323554001
