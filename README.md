# ATLAS_Workspace_analytics_infra

Infraestructura base para analytics con PostgreSQL en Docker.

## 1. Configuracion del contenedor

El proyecto usa un contenedor `postgres:16-alpine` con:

- Persistencia en volumen Docker (`postgres_data`).
- Variables de entorno en `.env`.
- `healthcheck` para validar disponibilidad.
- Red dedicada (`atlas_net`).
- Script de inicializacion SQL en `init-scripts/001_bootstrap.sql`.
- Montaje de `data/` para carga inicial de CSV.

## 2. Arranque

Desde la raiz del proyecto:

```bash
docker compose up -d
```

Verifica estado:

```bash
docker compose ps
docker compose logs -f atlas_postgres
```

## 3. Conexion desde DBeaver

Configura una conexion PostgreSQL con estos datos (tomados de `.env`):

- Host: `localhost`
- Puerto: `5432`
- Database: `atlas_analytics`
- User: `atlas_admin`
- Password: `ChangeThisStrongPassword`

## 4. Dataset cargado automaticamente

En el primer arranque, PostgreSQL ejecuta `init-scripts/001_bootstrap.sql` y carga:

- Tabla: `analytics.indian_developer_burnout_2026`
- Fuente: `data/indian_developer_burnout_2026.csv`

Consulta rapida:

```sql
SELECT COUNT(*)
FROM analytics.indian_developer_burnout_2026;
```

## 5. Buenas practicas recomendadas

- Cambiar la clave de `.env` antes de exponer el entorno.
- No versionar secretos reales.
- Usar backups periodicos del volumen `postgres_data`.
- Si modificas scripts de `init-scripts/`, recuerda que solo corren en una base nueva.