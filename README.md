# n8n-noco-grok

Este proyecto proporciona un entorno listo para usar con [n8n](https://n8n.io/), [PostgreSQL](https://www.postgresql.org/), [Qdrant](https://qdrant.tech/) y [Ngrok](https://ngrok.com/) usando Docker Compose. Es ideal para automatización de flujos de trabajo, almacenamiento de datos y exposición segura de endpoints a través de túneles.

## Servicios incluidos

- **n8n**: Plataforma de automatización de flujos de trabajo.
- **Postgres**: Base de datos relacional utilizada por n8n.
- **Qdrant**: Motor de vector search para IA y almacenamiento de embeddings.
- **Ngrok**: Exposición de servicios locales a Internet mediante túneles seguros.

## Requisitos previos

- [Docker](https://www.docker.com/) y [Docker Compose](https://docs.docker.com/compose/) instalados.
- Variables de entorno configuradas en un archivo `.env`:
  - `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`
  - `NGROK_TOKEN` (token de autenticación de Ngrok)
  - Opcional: `N8N_BASIC_AUTH_USER`, `N8N_BASIC_AUTH_PASSWORD` para proteger el acceso a n8n

## Uso rápido

1. Clona este repositorio y entra en la carpeta del proyecto.
2. Crea un archivo `.env` con las variables necesarias.
3. Levanta los servicios:
   ```sh
   docker-compose up -d
   ```
4. Accede a n8n en [http://localhost:5678](http://localhost:5678)
5. El endpoint público de n8n estará disponible mediante el dominio configurado en Ngrok.

## Archivos principales

- `docker-compose.yaml`: Define los servicios y redes.
- `ngrok.yml`: Configuración de túneles para exponer n8n.

## Personalización

- Modifica `ngrok.yml` para cambiar el dominio o configuración del túnel.
- Puedes añadir más servicios o cambiar los puertos según tus necesidades.

## Licencia

MIT
