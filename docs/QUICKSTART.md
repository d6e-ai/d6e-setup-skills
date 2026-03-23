# Quick Start - Deploy D6E in 5 Minutes

This guide assumes you have Docker installed and an external PostgreSQL database ready.

## 1. Clone and Configure

```bash
git clone https://github.com/d6e-ai/d6e-setup.git
cd d6e-setup
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
ORIGIN=https://example.d6e.ai

# D6E_AUTH_URL is pre-configured to https://www.d6e.ai
# Get CLIENT_ID and CLIENT_SECRET from the d6e-auth administrator
# (Contact them with your ORIGIN URL to register your instance)
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
example.d6e.ai {
	# SvelteKit-served API routes (skills docs). Must come before the general /api/v1 block.
	handle /api/v1/skills/* {
		reverse_proxy frontend:3000
	}

	# Rust API (e.g. same-origin /api/v1/.../files/.../download). Must not hit SvelteKit.
	handle /api/v1/* {
		reverse_proxy api:8080
	}

	handle {
		reverse_proxy frontend:3000
	}
}
EOF
```

Replace `example.d6e.ai` with your domain (DNS A record must point to this server). Set `ORIGIN` in `.env` to match your public URL. Caddy obtains Let’s Encrypt certificates automatically; optionally add `tls you@example.com` inside the site block to set the ACME contact email (omit if unnecessary).

## 5. Start

```bash
docker compose up -d
```

## 6. Verify

```bash
docker compose ps        # All services should be "running"
docker compose logs -f   # Check for errors
```

Open `https://example.d6e.ai` in your browser (use your real domain).

## Next Steps

- See the full [Setup Guide](../skills/d6e-setup/SKILL.md) for detailed instructions
- See the [Environment Variables Reference](../skills/d6e-setup/reference.md) for all configuration options
- See [D6E Docker STF Skills](https://github.com/d6e-ai/d6e-docker-stf-skills) to create custom workflow functions
