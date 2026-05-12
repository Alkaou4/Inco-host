# INCO.HOST

> Lightning fast image & video hosting — by **INCONNU BOY TECH**

Upload images and videos, get instant shareable URLs. No registration. No account. Just upload and go.

---

## Features

- **Auto host detection** — detects Heroku, Render, Railway, Fly.io, Koyeb, Vercel, local
- **Image & Video support** — JPG, PNG, GIF, WEBP, SVG, MP4, WEBM, MOV, MKV, and more
- **REST API** — integrate with bots, scripts, and apps
- **Multi-upload** — upload up to 10 files at once
- **Delete keys** — delete your files anytime with a secret key
- **Drag & Drop + Paste** — full clipboard paste support
- **Rate limiting** — built-in abuse protection
- **Zero config deploy** — works on Render & Heroku out of the box

---
### demo app live 

👉 : inco-host-production.up.railway.app
---

## Deploy

### Render (Recommended — Free Persistent Disk)

1. Fork/push this repo to GitHub
2. Go to [render.com](https://render.com) → New Web Service
3. Connect your repo
4. Build command: `npm install`
5. Start command: `node server.js`
6. Add a **Disk** under Advanced: mount at `/app/uploads`
7. Deploy — Render auto-sets `RENDER_EXTERNAL_URL`

Or use the one-click `render.yaml` (already included).

### Heroku

```bash
heroku create your-app-name
git push heroku main
```

Heroku auto-sets `HEROKU_APP_NAME` — base URL is detected automatically.

> ⚠️ Heroku free tier has ephemeral storage — files reset on dyno restart. Use a storage add-on (Cloudinary, S3) for persistence.

### Railway / Fly.io / Koyeb

Just deploy normally — the server auto-detects the platform from env variables.

---

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `PORT` | `3000` | Server port (auto-set by platforms) |
| `BASE_URL` | auto-detected | Override the base URL |
| `UPLOADS_DIR` | `./uploads` | Where files are stored |
| `MAX_IMAGE_SIZE` | `52428800` | Max image size in bytes (50MB) |
| `MAX_VIDEO_SIZE` | `524288000` | Max video size in bytes (500MB) |
| `RATE_LIMIT` | `30` | Max uploads per 15min per IP |

---

## API Reference

### Upload a file
```bash
POST /api/upload
Content-Type: multipart/form-data
Field: file

# Response
{
  "success": true,
  "url": "https://your-domain.com/f/uuid.jpg",
  "id": "uuid",
  "type": "image",
  "size": 204800,
  "deleteKey": "delete-uuid",
  "deleteUrl": "https://your-domain.com/api/delete/uuid?key=delete-uuid"
}
```

### Upload multiple files
```bash
POST /api/upload/multi
Content-Type: multipart/form-data
Field: files (up to 10)
```

### Get file info
```bash
GET /api/file/:id
```

### Delete a file
```bash
DELETE /api/delete/:id?key=deleteKey
```

### Platform info
```bash
GET /api/info
```

### Stats
```bash
GET /api/stats
```

---

## Usage in WhatsApp Bots (Baileys)

```javascript
const FormData = require('form-data');
const axios = require('axios');
const fs = require('fs');

async function uploadToIncoHost(filePath) {
  const form = new FormData();
  form.append('file', fs.createReadStream(filePath));
  
  const res = await axios.post('https://your-inco-host.onrender.com/api/upload', form, {
    headers: form.getHeaders()
  });
  
  return res.data.url; // Direct URL
}
```

---

## License

MIT — INCONNU BOY TECH
