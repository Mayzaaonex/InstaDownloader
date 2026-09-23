# InstaDownloader

Instagram downloader (Reel, Post, Photo) — scrape from [iqsaved.com](https://iqsaved.com/), output **pure JSON**.

---

## ✨ Features

- ✅ Download Instagram **Reel** (video)
- ✅ Download Instagram **Post** (photo)
- ✅ Get **caption**, **author username**, and **stats** (likes, comments)
- ✅ Get **thumbnail** for every media
- ✅ Output **pure JSON** — easy to parse
- ✅ Works on **PC** and **VPS**

---

## 📦 Dependencies

- **Node.js >= 18**
- [`socket.io-client`](https://www.npmjs.com/package/socket.io-client)

Install the dependency:

```bash
npm install socket.io-client
```

---

## 📥 Installation

```bash
git clone https://github.com/Mayzaaonex/InstaDownloader.git
cd InstaDownloader
npm install socket.io-client
```

---

## 🚀 Usage

```bash
node igdown.js "https://www.instagram.com/reel/Ddk-xYqRUTi/"
```

Works with reel, post, and IGTV links:

```bash
node igdown.js "https://www.instagram.com/p/XXXXXXXXXXX/"
node igdown.js "https://www.instagram.com/reel/XXXXXXXXXXX/"
```

---

## 🌐 Base URL

This scraper uses **[iqsaved.com](https://iqsaved.com/)** as the base URL for scraping Instagram content.

**Base URL:** `https://iqsaved.com/`

Media (video, photo, thumbnail) is served through IQSaved's own CDN.

---

## 📤 Example Output

### Success (Reel)

```json
{
  "creator": "Mayzaa",
  "status": true,
  "result": {
    "url": "https://www.instagram.com/reel/Ddk-xYqRUTi/",
    "platform": "instagram",
    "type": "reel",
    "author": {
      "username": "only_optimum",
      "avatar": null
    },
    "caption": "Apple cancelled the Mac Pro so I built my own 🤝 #apple",
    "stats": {
      "likes": 89881,
      "comments": 772,
      "views": null
    },
    "media": [
      {
        "type": "video",
        "thumbnail": "https://cdn.iqsaved.com/img.php?url=...",
        "url": "https://cdn.iqsaved.com/img.php?url=...",
        "filename": "video.mp4"
      }
    ],
    "video": "https://cdn.iqsaved.com/img.php?url=...",
    "image": null,
    "thumbnail": "https://cdn.iqsaved.com/img.php?url=..."
  }
}
```

### Error

```json
{
  "creator": "Mayzaa",
  "status": false,
  "message": "URL Instagram tidak valid"
}
```

### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `creator` | string | Script author name |
| `status` | boolean | `true` if success, `false` if failed |
| `result.url` | string | Original Instagram URL input |
| `result.platform` | string | Always `"instagram"` |
| `result.type` | string | `"reel"`, `"post"`, or `"igtv"` |
| `result.author.username` | string | Instagram author username |
| `result.caption` | string | Post/reel caption |
| `result.stats.likes` | number | Like count |
| `result.stats.comments` | number | Comment count |
| `result.stats.views` | number\|null | View count (if available) |
| `result.media` | array | List of media items (video/photo) |
| `result.media[].type` | string | `"video"` or `"photo"` |
| `result.media[].thumbnail` | string | Thumbnail URL |
| `result.media[].url` | string | Direct media URL |
| `result.media[].filename` | string | Suggested filename |
| `result.video` | string\|null | Shortcut to first video URL |
| `result.image` | string\|null | Shortcut to first photo URL |
| `result.thumbnail` | string\|null | Shortcut to first media thumbnail |
| `message` | string | Error message (if `status: false`) |

---

---

## ⚙️ Integration with Other Apps

### Node.js

```javascript
const { execSync } = require("child_process");

const output = execSync('node igdown.js "https://www.instagram.com/reel/Ddk-xYqRUTi/"').toString();
const data = JSON.parse(output);

console.log(data.result.video || data.result.image); // media URL
console.log(data.result.thumbnail); // thumbnail URL
```

### Express.js (as API)

```javascript
const express = require("express");
const { execSync } = require("child_process");
const app = express();

app.get("/api/instagram", (req, res) => {
  const url = req.query.url;
  if (!url) return res.status(400).json({ error: "URL required" });

  try {
    const output = execSync(`node igdown.js "${url}"`, { timeout: 60000 }).toString();
    res.type("application/json").send(output);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.listen(3000, () => console.log("Server running at http://localhost:3000"));
```

Call: `http://localhost:3000/api/instagram?url=https://www.instagram.com/reel/Ddk-xYqRUTi/`

---

## ⚠️ Important Notes

1. **Public accounts only** — private Instagram accounts cannot be scraped.
2. **Rate limit** — do not spam. Add a delay between requests.
3. **Media URLs are proxied** through IQSaved's CDN and may expire or change over time.
4. **Disclaimer** — Use only for content you have rights to or that is permitted. Respect Instagram creator copyright and privacy.

---

## 📝 License

MIT License

Copyright (c) 2026 Mayzaa

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## 👤 Author

**Mayzaa**
- GitHub: [@Mayzaaonex](https://github.com/Mayzaaonex)
- Repository: [InstaDownloader](https://github.com/Mayzaaonex/InstaDownloader)

---

## 🙏 Credits

- [iqsaved.com](https://iqsaved.com/) — Instagram scraper backend (base URL)
