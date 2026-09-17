# n8n-noco-grok

Entorno self-hosted para [n8n](https://n8n.io/) con PostgreSQL, Qdrant, Ngrok y el sandbox oficial de n8n para `n8n Assistant`.

## Servicios

- **n8n**: automatización de workflows y n8n Assistant.
- **PostgreSQL**: persistencia de n8n.
- **Qdrant**: almacenamiento vectorial para IA.
- **sandbox-certs**, **sandbox-api** y **sandbox-runner-1**: ejecución aislada de código mediante el sandbox oficial de n8n.
- **runners**: task runners externos de n8n.
- **SearXNG**: búsqueda web interna para Assistant.
- **Ngrok**: endpoint público para webhooks y conexiones externas.

## Requisitos

- Docker Engine y Docker Compose v2.
- Al menos 4 GB de RAM y 2 vCPUs para el runner Docker-in-Docker.
- Un token de Ngrok.
- Una API key de Anthropic, OpenAI u OpenRouter.

## Configuración inicial

1. Copia la plantilla de variables:

   ```sh
   cp .env.example .env
   ```

2. Completa `.env` con tus credenciales y secretos únicos. No subas este archivo a GitHub.

3. Crea la configuración local de Ngrok:

   ```sh
   cp ngrok.example.yml ngrok.yml
   ```

   Edita `ngrok.yml` y sustituye `https://your-subdomain.ngrok-free.app` por tu endpoint privado. Este archivo está ignorado por Git; `ngrok.example.yml` es el único que debe versionarse.

4. Inicia el entorno:

   ```sh
   docker compose up -d
   docker compose ps
   ```

5. Abre n8n en [http://localhost:5678](http://localhost:5678).

## n8n Assistant

`.env.example` activa `instance-ai` y conecta Assistant al sandbox oficial mediante `http://sandbox-api:8080`. El proveedor, la API key y el modelo se configuran desde la interfaz de n8n, en los ajustes del asistente.

Completa estas variables antes de usarlo:

- `N8N_RUNNERS_AUTH_TOKEN`.
- `SANDBOX_API_KEYS`.
- `SANDBOX_API_RUNNER_REGISTRATION_TOKEN`.
- `SANDBOX_API_RUNNER_API_KEY`.
- `N8N_SANDBOX_SERVICE_API_KEY`, con un valor incluido en `SANDBOX_API_KEYS`.
- `SEARXNG_SECRET`.

La búsqueda web usa SearXNG. `INSTANCE_AI_BRAVE_SEARCH_API_KEY` es opcional y tiene prioridad si se configura.

Comprueba el sandbox con:

```sh
docker compose exec n8n wget -qO- http://sandbox-api:8080/healthz
```

Después de cambiar `.env`, recrea n8n:

```sh
docker compose up -d --force-recreate n8n
```

Assistant está en Preview. Revisa los workflows generados antes de utilizarlos en producción.

## URL pública y Ngrok

`WEBHOOK_URL` debe coincidir con el endpoint HTTPS configurado en tu `ngrok.yml` cuando uses webhooks o canales externos.

El token se proporciona mediante `NGROK_TOKEN` en `.env`; no lo escribas en ningún archivo YAML versionable.

## Archivos principales

- `docker-compose.yaml`: servicios, redes, persistencia y configuración.
- `.env.example`: plantilla de variables sin credenciales reales.
- `ngrok.example.yml`: configuración de Ngrok para versionar.
- `ngrok.yml`: configuración local con tu endpoint privado; ignorada por Git.
- `searxng-settings.yml`: habilita el formato JSON que usa Assistant.

## Seguridad

No publiques los puertos de `sandbox-api` ni `sandbox-runner-1`. El runner es privilegiado porque usa Docker-in-Docker y debe permanecer accesible solo dentro de la red de Compose.

## Licencia

MIT
