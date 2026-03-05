# Quick Start - Deploy D6E in 5 Minutes

This guide assumes you have Docker installed and an external PostgreSQL database ready.

## 1. Clone and Configure

```bash
git clone https://github.com/d6e-ai/d6e.git
cd d6e
cp .env.example .env
```

## 2. Edit .env

Set these required values in `.env`:

```bash
# Your external database
DATABASE_URL=postgres://user:password@your-db-host:5432/d6e

# Generate with: openssl rand -base64 32
D6E_CONTAINER_TOKEN_SECRET=<generated-value>

# Your public URL
ORIGIN=https://your-domain.example.com

# Get these from the d6e-auth administrator
# (Contact them with your ORIGIN URL to register your instance)
D6E_AUTH_URL=https://auth.d6e.ai
D6E_AUTH_CLIENT_ID=d6e_xxxxxxxxxxxx
D6E_AUTH_CLIENT_SECRET=d6es_xxxxxxxxxxxx
```

## 3. Apply Database Schema

```bash
psql $DATABASE_URL < packages/migration/seed.sql
```

Apply seed data (requires Node.js):

```bash
cd packages/migration && npm install pg
DATABASE_URL="postgres://..." node scripts/seed_fonts.mjs
DATABASE_URL="postgres://..." node scripts/seed_libraries.mjs
cd ../..
```

## 4. Create Caddyfile

```bash
cat > Caddyfile << 'EOF'
your-domain.example.com {
    reverse_proxy frontend:3000
}
EOF
```

Replace `your-domain.example.com` with your actual domain (DNS A record must point to this server).

## 5. Start

```bash
docker compose up -d
```

## 6. Verify

```bash
docker compose ps        # All services should be "running"
docker compose logs -f   # Check for errors
```

Open `https://your-domain.example.com` in your browser.

## Next Steps

- See the full [Setup Guide](../skills/d6e-setup/SKILL.md) for detailed instructions
- See the [Environment Variables Reference](../skills/d6e-setup/reference.md) for all configuration options
- See [D6E Docker STF Skills](https://github.com/d6e-ai/d6e-docker-stf-skills) to create custom workflow functions
