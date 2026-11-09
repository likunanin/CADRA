# CADRA — AI Creative Studio website

## Run
1. Install Node.js 18+ (https://nodejs.org)
2. In this folder:
   ADMIN_PASSWORD=your-secret node server.js      (Windows PowerShell: $env:ADMIN_PASSWORD="your-secret"; node server.js)
3. Open http://localhost:3000  — orders page: http://localhost:3000/admin.html

## Structure
- public/index.html   website (design + order form)
- public/admin.html   order dashboard (password protected API)
- server.js           backend: saves orders to data/orders.json
- data/               orders are stored here (back this up)

## Optional: Telegram alert for each new order
Set TELEGRAM_BOT_TOKEN and TELEGRAM_CHAT_ID (create a bot with @BotFather).
The admin dashboard includes a **Test Telegram** button. Configure both values, open `/admin.html`, enter the admin password, and click it to confirm delivery. Failed sends are reported in the server console and in the dashboard.

## Order management
In `/admin.html`, open the dashboard to filter orders by status, update an order's status (new, contacted, in progress, or done), see the new-order count, and export all orders to CSV. Order data is written atomically to `data/orders.json`; automatic daily copies are kept in `data/backups/` (up to 14).

Use `.env.example` as the environment-variable checklist and replace the placeholder admin password. This dependency-free server does not load `.env` automatically; set variables in your shell or hosting dashboard. In PowerShell, for example: `$env:ADMIN_PASSWORD="your-secret"; $env:TELEGRAM_BOT_TOKEN="your-token"; $env:TELEGRAM_CHAT_ID="your-chat-id"; node server.js`. Keep `.env` and `data/` out of source control; the included `.gitignore` excludes both. JSON storage is appropriate for a small studio with modest order volume; make sure the `data/` directory is on persistent storage when deploying.

## Deploy
### Deploy to Render
1. Push this project to GitHub.
2. In the Render dashboard, choose **New → Blueprint**, connect the repository, and deploy using `render.yaml`.
3. When prompted, enter a strong `ADMIN_PASSWORD`. In the service's **Environment** settings, add `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` if you want Telegram alerts. Never commit real values for these variables.
4. The blueprint creates a Node web service on the Starter plan and mounts a 1 GB persistent disk at `/data`. It sets `DATA_DIR=/data`, so order JSON and daily backups are stored on that disk. Render supplies `PORT`; the server listens on it.
5. After deploy, open `https://<your-render-service>.onrender.com/health` and confirm it returns `ok`. Then open `/admin.html`, sign in, and use **Test Telegram** if notifications are configured.
6. To use `cadra.ge`, add the custom domain in Render, then add the DNS records Render specifies at your domain registrar. Render provisions HTTPS after DNS verification. Set `SITE_URL=https://cadra.ge` in the Render environment once the domain is connected.

The server also works on other Node hosts. Set `DATA_DIR` to the host's persistent-storage mount path, `SITE_URL` to the canonical origin (for example, `https://cadra.ge`), and the required admin password in the host's environment settings. If the domain changes, update the canonical, hreflang, Open Graph, and Twitter URLs in `public/index.html`. Add the social preview image at `public/og-image.jpg`; the page currently references that path.

The site uses `public/logo.png` for its favicon and Apple touch icon. Static assets are cached for one day; HTML is revalidated so updates appear promptly.

Phone: +995 593 415 541 (edit in public/index.html, search "593").
