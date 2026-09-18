# Subra Companion

Subra Companion is a private, local-first personal AI assistant developed by [Subrata](https://www.linkedin.com/in/subraatakumar/) with the help of AI.

Its assistant, **Subra AI**, provides a general chat interface called the **Global Brain**. The project will grow over time through explicit tools for personal information and task automation while keeping the application single-owner and privacy-focused.

The source repository is private. The runnable container image and this documentation are public.

## Current capabilities

- Chat with locally installed Ollama models
- Automatically discover available Ollama models
- Switch models from the chat interface
- Stream responses
- Stop, regenerate, and copy responses
- Run locally on port `8200`

Personal-data tools and browser automation are not available yet. Subra AI will not invent agent counts, job-application counts, or completed actions when their real data sources are not connected.

## Requirements

- Docker Desktop or Docker Engine
- [Ollama](https://ollama.com/) running on the host
- At least one downloaded Ollama model

For example:

```bash
ollama pull llama3.2
```

## Run with Docker Compose

Save the following as `compose.yaml`:

```yaml
services:
  companion:
    image: ghcr.io/subraatakumar/subra-ai-companion:latest
    container_name: subra-ai-companion
    restart: unless-stopped
    ports:
      - "127.0.0.1:8200:8200"
    volumes:
      - subra-assistant-data:/data
    environment:
      OLLAMA_BASE_URL: http://host.docker.internal:11434
    extra_hosts:
      - "host.docker.internal:host-gateway"

volumes:
  subra-assistant-data:
```

Start it:

```bash
docker compose up -d
```

Open [http://localhost:8200](http://localhost:8200).

## Run directly

```bash
docker run -d \
  --name subra-ai-companion \
  --restart unless-stopped \
  -p 127.0.0.1:8200:8200 \
  --add-host host.docker.internal:host-gateway \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v subra-assistant-data:/data \
  ghcr.io/subraatakumar/subra-ai-companion:latest
```

## Privacy and security

- The application binds to `127.0.0.1` by default and is not publicly exposed.
- Chat requests are sent to the configured Ollama service.
- No cloud AI provider is enabled by default.
- Do not expose port `8200` directly to the public internet.
- Treat the `/data` volume as private and include it in backups only when the backup is protected.

The current release does not yet persist chats, operate a browser, or store website sessions.

## Updating

```bash
docker compose pull
docker compose up -d
```

For reproducible deployments, use a version tag instead of `latest` after versioned releases become available.

## Publishing (maintainer only)

The private source repository includes both a GitHub Actions release workflow and a local Docker Buildx publishing script. The local script produces the same multi-architecture image tags, provenance, and software bill of materials without requiring a GitHub-hosted runner. It requires a GitHub token with `write:packages` permission; that token is supplied at runtime and is never stored in the repository or container image.

## Troubleshooting

If the application shows **Ollama offline**:

1. Confirm Ollama is running on the host.
2. Confirm `http://localhost:11434/api/tags` responds on the host.
3. Confirm the container uses `OLLAMA_BASE_URL=http://host.docker.internal:11434`.
4. On Linux, retain the `host.docker.internal:host-gateway` mapping shown above.

## Project status

This is an early development release. The public container is intended for personal local use. New capabilities will be documented only after they are implemented and verified.

## About

Subra AI is the Global Brain of the Subra Companion app, developed by Subrata with the help of AI.
