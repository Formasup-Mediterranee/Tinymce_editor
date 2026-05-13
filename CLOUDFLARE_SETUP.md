# Free Public Domain Setup - LocalTunnel

## 🎉 Zero Configuration Required!

Your setup automatically generates a **free public domain** every time you start the app.

## Quick Start

```bash
docker-compose up
```

That's it! 🚀

## What Happens

1. n8n starts on your VM (54.38.224.140:5678)
2. LocalTunnel automatically creates a public URL
3. Check the logs for your generated domain:

```
localtunel | your-n8n-app-xxxx.loca.lt ← YOUR PUBLIC DOMAIN
```

## Access Your n8n

- **Public domain:** `https://your-n8n-app-xxxx.loca.lt` (auto-generated in logs)
- **Direct IP:** `http://54.38.224.140:5678`

## How It Works

- **LocalTunnel**: Completely free, no sign-up required
- **Auto-generated domain**: New domain each time you start
- **Public access**: Share the domain with anyone
- **Unlimited bandwidth**: No rate limits on free tier

## View Logs

To see your generated domain:

```bash
# Show all logs
docker-compose logs

# Follow n8n tunnel logs
docker-compose logs -f localtunel
```

## Stop Services

```bash
docker-compose down
```

## Tips

- Domain changes each restart (this is normal)
- To keep the same domain, note it down and recreate it
- The tunnel is active as long as containers are running
- Perfect for development and testing!

That's it - completely free and automatic! ✨
