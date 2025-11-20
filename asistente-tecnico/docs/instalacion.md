# Guía de Instalación y Despliegue

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Docker** (versión 20.10 o superior)
- **Docker Compose** (versión 2.0 o superior)
- **Git** (para clonar el repositorio)
- Al menos **2GB de RAM** disponible
- **5GB de espacio en disco**

## 🚀 Instalación Paso a Paso

### 1. Clonar el Repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd <NOMBRE_DEL_PROYECTO>
```

### 2. Configurar Variables de Entorno

Crea el archivo `.env` en la raíz del proyecto:

```bash
cp .env.example .env
```

Edita el archivo `.env` con tus credenciales:

```bash
nano .env
```

**Variables requeridas:**

```env
# PostgreSQL
POSTGRES_USER=tu_usuario
POSTGRES_PASSWORD=tu_password_seguro
POSTGRES_DB=n8n_db

# n8n
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=tu_password_n8n
N8N_ENCRYPTION_KEY=genera_una_clave_aleatoria_aqui

# OpenAI
OPENAI_API_KEY=tu_api_key_de_openai

# Telegram
TELEGRAM_BOT_TOKEN=tu_token_de_telegram

# PgAdmin
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=tu_password_pgadmin

# Grafana
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=tu_password_grafana
```

> ⚠️ **IMPORTANTE**: Nunca subas el archivo `.env` al repositorio. Ya está incluido en `.gitignore`.

### 3. Crear Directorios de Persistencia

```bash
mkdir -p data/n8n_data
mkdir -p data/postgres_data
mkdir -p data/pgadmin_data
mkdir -p data/grafana_data
```

### 4. Desplegar los Servicios

Levanta todos los contenedores:

```bash
docker-compose up -d
```

Verifica que todos los servicios estén corriendo:

```bash
docker-compose ps
```

### 5. Verificar los Logs

Para verificar que todo funciona correctamente:

```bash
# Ver logs de todos los servicios
docker-compose logs -f

# Ver logs de un servicio específico
docker-compose logs -f n8n
docker-compose logs -f postgres
```

## 🌐 Acceso a los Servicios

Una vez desplegados, puedes acceder a:

| Servicio | URL | Credenciales |
|----------|-----|--------------|
| **n8n** | http://localhost:5678 | Usuario y contraseña del `.env` |
| **PgAdmin** | http://localhost:5050 | Email y contraseña del `.env` |
| **Grafana** | http://localhost:3000 | Usuario y contraseña del `.env` |
| **PostgreSQL** | localhost:5432 | Usuario y contraseña del `.env` |

## 📦 Importar Workflows

### Opción 1: Desde la Interfaz de n8n

1. Accede a n8n en http://localhost:5678
2. Ve a **Workflows** → **Import from File**
3. Selecciona el archivo `workflows/main_agent.json`
4. Haz clic en **Import**

### Opción 2: Usando la API de n8n

```bash
curl -X POST http://localhost:5678/api/v1/workflows \
  -H "Content-Type: application/json" \
  -u "usuario:password" \
  -d @workflows/main_agent.json
```

## 🔧 Configuración Post-Instalación

### Configurar Conexión a PostgreSQL en PgAdmin

1. Accede a PgAdmin (http://localhost:5050)
2. Click derecho en **Servers** → **Register** → **Server**
3. En la pestaña **General**:
   - Name: `n8n_postgres`
4. En la pestaña **Connection**:
   - Host: `postgres` (nombre del servicio en Docker)
   - Port: `5432`
   - Database: El valor de `POSTGRES_DB` del `.env`
   - Username: El valor de `POSTGRES_USER` del `.env`
   - Password: El valor de `POSTGRES_PASSWORD` del `.env`
5. Click en **Save**

### Activar el Workflow Principal

1. En n8n, abre el workflow importado
2. Verifica que todas las credenciales estén configuradas:
   - OpenAI API Key
   - Telegram Bot Token
   - Conexión a PostgreSQL
3. Activa el workflow usando el switch en la esquina superior derecha

## 🛑 Comandos Útiles

### Detener los Servicios

```bash
docker-compose down
```

### Reiniciar los Servicios

```bash
docker-compose restart
```

### Ver Logs en Tiempo Real

```bash
docker-compose logs -f
```

### Eliminar Todo (Incluyendo Volúmenes)

```bash
docker-compose down -v
```

> ⚠️ **ADVERTENCIA**: Este comando eliminará todos los datos persistentes.

### Actualizar los Servicios

```bash
docker-compose pull
docker-compose up -d
```

## 🔒 Backup y Restauración
### Realizar Backup de PostgreSQL

```bash
docker-compose exec postgres pg_dump -U $POSTGRES_USER $POSTGRES_DB > backup_$(date +%Y%m%d_%H%M%S).sql
```

### Restaurar Backup

```bash
docker-compose exec -T postgres psql -U $POSTGRES_USER $POSTGRES_DB < backup_20240101_120000.sql
```

### Backup de Workflows de n8n

Los workflows se guardan automáticamente en `data/n8n_data/`. También puedes exportarlos manualmente desde la interfaz y guardarlos en `workflows/`.

## 🐛 Solución de Problemas

### Error: Puerto ya en uso

Si algún puerto está ocupado, modifica los puertos en `docker-compose.yml`:

```yaml
ports:
  - "5679:5678"  # Cambiar el puerto del host
```

### n8n no se conecta a PostgreSQL

Verifica que el servicio de PostgreSQL esté corriendo:

```bash
docker-compose logs postgres
```

Asegúrate de que las credenciales en `.env` sean correctas.

### Problemas de Permisos

Si tienes problemas de permisos en los directorios de datos:

```bash
sudo chown -R $USER:$USER data/
```

### Contenedor se reinicia constantemente

Revisa los logs del contenedor problemático:

```bash
docker-compose logs [nombre_servicio]
```

## 📚 Recursos Adicionales

- [Documentación de n8n](https://docs.n8n.io/)
- [Documentación de Docker Compose](https://docs.docker.com/compose/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

## 🆘 Soporte

Si encuentras algún problema durante la instalación:

1. Revisa los logs de los contenedores
2. Verifica que todas las variables de entorno estén configuradas correctamente
3. Asegúrate de cumplir con los requisitos previos
4. Consulta la sección de solución de problemas

---

**Última actualización**: Noviembre 2025
