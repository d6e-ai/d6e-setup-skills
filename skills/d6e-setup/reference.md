# D6E Setup - Environment Variables Reference

Complete reference for all environment variables used in D6E deployment.

## Database Configuration

| Variable            | Required | Default    | Description                                                              |
| ------------------- | -------- | ---------- | ------------------------------------------------------------------------ |
| `DATABASE_URL`      | Yes      | -          | PostgreSQL connection string (e.g. `postgres://user:pass@host:5432/d6e`) |
| `POSTGRES_USER`     | No       | `postgres` | PostgreSQL username (only used with `compose.withdb.yml`)                |
| `POSTGRES_PASSWORD` | No       | -          | PostgreSQL password (only used with `compose.withdb.yml`)                |
| `POSTGRES_DB`       | No       | `d6e`      | PostgreSQL database name (only used with `compose.withdb.yml`)           |
| `POSTGRES_PORT`     | No       | `5432`     | PostgreSQL port (only used with `compose.withdb.yml`)                    |

## Service Ports

| Variable        | Required | Default | Description          |
| --------------- | -------- | ------- | -------------------- |
| `API_PORT`      | No       | `8080`  | API server host port |
| `MCP_PORT`      | No       | `8081`  | MCP server host port |
| `FRONTEND_PORT` | No       | `3000`  | Frontend host port   |

## Internal Service URLs

These are used for inter-container communication and typically do not need to be changed.

| Variable             | Required | Default                               | Description                      |
| -------------------- | -------- | ------------------------------------- | -------------------------------- |
| `D6E_API_URL`        | No       | `http://api:8080`                     | API URL used by frontend and MCP |
| `D6E_MCP_SERVER_URL` | No       | `http://mcp:8081/mcp`                 | MCP server URL used by frontend  |
| `OLLAMA_BASE_URL`    | No       | `http://host.docker.internal:11434`   | Ollama API URL (host machine)    |
| `LM_STUDIO_BASE_URL` | No       | `http://host.docker.internal:1234/v1` | LM Studio API URL (host machine) |

## Authentication (d6e-auth)

| Variable                 | Required | Default    | Description                                          |
| ------------------------ | -------- | ---------- | ---------------------------------------------------- |
| `D6E_AUTH_URL`           | Yes      | `https://www.d6e.ai` | d6e-auth server URL                    |
| `D6E_AUTH_CLIENT_ID`     | Yes      | -          | OAuth client ID issued by d6e-auth administrator     |
| `D6E_AUTH_CLIENT_SECRET` | Yes      | -          | OAuth client secret issued by d6e-auth administrator |
| `D6E_AUTH_JWT_ISSUER`    | No       | `d6e-auth` | JWT issuer claim for token validation                |

### How to Obtain d6e-auth Credentials

1. Determine your D6E instance's public URL (e.g. `https://example.d6e.ai`)
2. Contact the d6e-auth administrator with this URL
3. The administrator will register your instance using the `register-client.js` script:

   ```bash
   node scripts/register-client.js "your-org-name" \
     --base-url "https://example.d6e.ai"
   ```

4. You will receive `D6E_AUTH_CLIENT_ID` and `D6E_AUTH_CLIENT_SECRET` values
5. Set these in your `.env` file (`D6E_AUTH_URL` is already set to `https://www.d6e.ai` by default)

## Security

| Variable                     | Required | Default | Description                                                                                                 |
| ---------------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------------- |
| `D6E_CONTAINER_TOKEN_SECRET` | Yes      | -       | Secret key for signing Docker container auth tokens. Generate with `openssl rand -base64 32`                |
| `ORIGIN`                     | Yes      | -       | Public URL of D6E instance (e.g. `https://example.d6e.ai`). Used by SvelteKit and OAuth redirect validation |

## LLM API Keys (Optional)

These are only needed if you want to use AI features.

| Variable            | Required | Default | Description                      |
| ------------------- | -------- | ------- | -------------------------------- |
| `OPENAI_API_KEY`    | No       | -       | OpenAI API key (`sk-...`)        |
| `ANTHROPIC_API_KEY` | No       | -       | Anthropic API key (`sk-ant-...`) |
| `GOOGLE_API_KEY`    | No       | -       | Google AI API key                |

## Embedding Configuration (Optional)

Required only for similarity search features. `GOOGLE_API_KEY` must also be set.

| Variable               | Required | Default                | Description                 |
| ---------------------- | -------- | ---------------------- | --------------------------- |
| `EMBEDDING_MODEL`      | No       | `gemini-embedding-001` | Embedding model name        |
| `EMBEDDING_DIMENSIONS` | No       | `768`                  | Embedding vector dimensions |

## Minimal .env Example

The absolute minimum `.env` for a production deployment:

```bash
# Database (external)
DATABASE_URL=postgres://user:password@db-host:5432/d6e

# Security
D6E_CONTAINER_TOKEN_SECRET=<openssl rand -base64 32>
ORIGIN=https://example.d6e.ai

# Authentication
D6E_AUTH_URL=https://www.d6e.ai
D6E_AUTH_CLIENT_ID=d6e_xxxxxxxxxxxx
D6E_AUTH_CLIENT_SECRET=d6es_xxxxxxxxxxxx
```

## Docker Compose Files

| File          | Use Case                 | Description                                        |
| ------------- | ------------------------ | -------------------------------------------------- |
| `compose.yml` | Production (external DB) | Uses pre-built images from ghcr.io, includes Caddy |
