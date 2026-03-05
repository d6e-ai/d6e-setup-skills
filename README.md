# D6E Setup Skills

[![Skills](https://img.shields.io/badge/skills.sh-d6e--setup--skills-blue)](https://skills.sh)
[![GitHub](https://img.shields.io/github/stars/d6e-ai/d6e-setup-skills?style=social)](https://github.com/d6e-ai/d6e-setup-skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Claude/Cursor Agent Skills for deploying and configuring D6E platform instances.

## What is This?

This repository contains **Agent Skills** that teach Claude and Cursor how to help users set up and deploy [D6E](https://github.com/d6e-ai/d6e) — an AI-native Business Intelligence platform.

### What are Agent Skills?

Agent Skills are Markdown documents that provide AI assistants with domain-specific knowledge and workflows. They enable Claude/Cursor to:

- Guide users through the complete D6E setup process
- Configure environment variables correctly
- Set up HTTPS with Caddy reverse proxy
- Apply database schemas and seed data
- Troubleshoot common deployment issues

## Available Skills

### [D6E Setup](./skills/d6e-setup/SKILL.md)

Teaches Claude/Cursor how to help users deploy D6E, including:

- Cloning the repository and configuring `.env`
- Setting up external PostgreSQL database connection
- Obtaining and configuring d6e-auth credentials
- Applying database schema (`seed.sql`) and seed data
- Creating a Caddyfile for automatic HTTPS
- Starting services with Docker Compose
- Verifying deployment and troubleshooting

## Installation

### Quick Install (Recommended)

Install this skill using the skills.sh CLI:

```bash
npx skills add d6e-ai/d6e-setup-skills
```

This will automatically set up the skill in your Cursor environment.

### Manual Installation

#### For Cursor

1. **Clone the repository:**

   ```bash
   git clone https://github.com/d6e-ai/d6e-setup-skills.git
   ```

2. **Add to Cursor:**
   - Open Cursor Settings (Cmd/Ctrl + ,)
   - Navigate to "Features" → "Agent Skills"
   - Add skill directory: `/path/to/d6e-setup-skills/skills/d6e-setup`

3. **Verify installation:**
   - Open Composer (Cmd/Ctrl + I)
   - Type `@skills` to see available skills
   - You should see "d6e-setup"

#### For Claude Code

1. **Clone the repository:**

   ```bash
   git clone https://github.com/d6e-ai/d6e-setup-skills.git
   ```

2. **Reference the skill:**
   - In your project, reference `@skills/d6e-setup/SKILL.md`
   - Claude will automatically load the skill content

## How to Use

### For Cursor Users

1. **Open Composer (Cmd/Ctrl + I)**

2. **Paste a prompt:**

   ```
   Using the D6E Setup skill, help me deploy D6E on my server.

   Details:
   - Server: Ubuntu 24.04
   - Database: postgres://user:pass@db.example.com:5432/d6e
   - Domain: analytics.example.com
   ```

3. **Follow the guided steps**

### For Claude Code Users

1. **Reference the skill document:**

   ```
   Using @skills/d6e-setup/SKILL.md, set up D6E with my external PostgreSQL database.
   ```

2. **Follow the guided steps**

### Example Prompts

#### Full Setup

```
Using the D6E Setup skill, deploy D6E on my Ubuntu server.

- Domain: analytics.mycompany.com
- Database: postgres://d6e:password@rds.amazonaws.com:5432/d6e
- I have Docker installed already
```

#### Environment Configuration Only

```
Using the D6E Setup skill, help me configure the .env file for D6E.
I have the repository cloned already.
```

#### HTTPS Setup Only

```
Using the D6E Setup skill, help me set up HTTPS with Caddy for my D6E instance.
My domain is analytics.mycompany.com.
```

#### Troubleshooting

```
Using the D6E Setup skill, my D6E instance can't connect to the database.
Here's the error from docker compose logs...
```

## Documentation

- **[Quick Start](./docs/QUICKSTART.md)** - Deploy D6E in 5 minutes
- **[Skill Document](./skills/d6e-setup/SKILL.md)** - Complete setup guide
- **[Environment Variables Reference](./skills/d6e-setup/reference.md)** - All configuration options

## Related Resources

- [D6E Platform](https://github.com/d6e-ai/d6e) - D6E main repository
- [D6E Documentation](https://github.com/d6e-ai/d6e/tree/main/docs) - Full documentation
- [D6E Docker STF Skills](https://github.com/d6e-ai/d6e-docker-stf-skills) - Skills for creating custom Docker STFs
- [skills.sh](https://skills.sh) - The Open Agent Skills Ecosystem

## Contributing

Contributions are welcome! Help us improve the setup experience.

### What You Can Contribute

- Documentation improvements (typo fixes, clarifications)
- New troubleshooting scenarios
- Platform-specific setup instructions (e.g. AWS, GCP, Azure)
- Bug reports and fixes

### Simple Contribution Steps

1. **Fork the repository**
2. **Make changes**
3. **Create a Pull Request**

## License

MIT License - see [LICENSE](LICENSE) for details
