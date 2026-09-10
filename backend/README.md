# Portfolio Contact Form Backend

A tiny Express server with one job: receive submissions from your portfolio's
contact form and email them to you. GitHub Pages (where your portfolio is
hosted) can only serve static files, so this backend has to run somewhere
else — free options below.

## What it does

`POST /api/contact` with JSON `{ name, email, message }` → sends you an email
with the message and the sender's reply-to address set, so you can just hit
"Reply" in Gmail.

## 1. Get a Gmail App Password

1. Turn on 2-Step Verification on the Google account you want to send from:
   https://myaccount.google.com/security
2. Create an App Password: https://myaccount.google.com/apppasswords
3. Copy the 16-character password — you'll need it in step 3 below.

## 2. Deploy the backend (Render.com free tier — no credit card needed)

1. Push this `backend` folder to its own GitHub repo (or a `backend/` folder
   in your existing repo — either works).
2. Go to https://render.com → New → Web Service → connect your repo.
3. Settings:
   - Root Directory: `backend` (if it's a subfolder)
   - Build Command: `npm install`
   - Start Command: `npm start`
4. Add environment variables (Render dashboard → Environment):
   - `EMAIL_USER` = your Gmail address
   - `EMAIL_PASS` = the App Password from step 1
   - `ALLOWED_ORIGIN` = `https://vishal-dev123.github.io`
5. Deploy. Render will give you a URL like
   `https://vishal-portfolio-backend.onrender.com`.

(Railway.app and Fly.io work the same way if you prefer those.)

## 3. Connect the frontend

Open `index.html`, find this line near the bottom:

```js
const CONTACT_API_URL = 'https://YOUR-BACKEND-URL.onrender.com/api/contact';
```

Replace it with your real Render URL + `/api/contact`, e.g.:

```js
const CONTACT_API_URL = 'https://vishal-portfolio-backend.onrender.com/api/contact';
```

Commit and push. The form will now send real emails. If the backend is ever
down or not yet deployed, the form automatically falls back to opening the
visitor's email app instead — so it never fully breaks.

## Running locally (optional, to test before deploying)

```bash
cd backend
cp .env.example .env      # then fill in your real values
npm install
npm start
```

Server runs at `http://localhost:3000`. Point `CONTACT_API_URL` in
`index.html` to `http://localhost:3000/api/contact` while testing.

## Note on Render's free tier

Free services spin down after inactivity and take ~30–50 seconds to wake up
on the next request. That's fine for a portfolio contact form — the visitor
just waits a moment longer, and the button shows "Sending…" the whole time.
