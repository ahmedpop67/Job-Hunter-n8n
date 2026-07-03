# Job Hunter — Setup Steps

## 1. Prerequisites
- Docker + Docker Compose installed
- ngrok installed with an authtoken configured (`ngrok config add-authtoken <token>`)
- Telegram bot token (from @BotFather)
- Google Cloud OAuth2 app (Drive + Gmail scopes)
- SerpApi key
- OpenRouter key

## 2. Folder structure
```
job-hunter/
├── docker-compose.yml
├── .env
├── local-files/
└── 1783098445768_Job_Hunter_1_.json
```

## 3. Configure `.env`
Fill in:
```
POSTGRES_USER=n8n
POSTGRES_PASSWORD=<your_password>
POSTGRES_DB=n8n

N8N_HOST=<your-ngrok-domain>.ngrok-free.dev
N8N_PROTOCOL=https
N8N_PORT=5678
WEBHOOK_URL=https://<your-ngrok-domain>.ngrok-free.dev/
N8N_EDITOR_BASE_URL=https://<your-ngrok-domain>.ngrok-free.dev/
GENERIC_TIMEZONE=Africa/Cairo

N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=<your_password>

N8N_ENCRYPTION_KEY=<openssl rand -hex 32>
```

## 4. Start ngrok tunnel
```bash
ngrok http --url=<your-ngrok-domain>.ngrok-free.dev 5678
```
Keep this running. Confirm the domain matches `.env` exactly.

## 5. Start the stack
```bash
cd job-hunter
docker compose up -d
docker compose logs -f n8n
```

## 6. Open n8n
```
https://<your-ngrok-domain>.ngrok-free.dev
```
Log in with `N8N_BASIC_AUTH_USER` / `N8N_BASIC_AUTH_PASSWORD`.

## 7. Import workflow
1. Workflows → Import from File → `1783098445768_Job_Hunter_1_.json`.
2. Create credentials:
   - Telegram account (bot token)
   - Gmail account (OAuth2, redirect URI: `https://<your-ngrok-domain>.ngrok-free.dev/rest/oauth2-credential/callback`)
   - Google Drive account (OAuth2, same redirect URI)
   - OpenRouter account (API key)
   - SerpApi account (API key)
3. In **Download file** node → replace Google Drive file URL with your resume.
4. In **Details** node → set `expected_salary` to your real value.
5. In **Send a message** node → set `sendTo` to your email.
6. Activate the workflow.

## 8. Verify Telegram webhook
```bash
curl "https://api.telegram.org/bot<BOT_TOKEN>/getWebhookInfo"
```

## 9. Test
Send in Telegram:
```
/help
/oran
```
