# Chrome

Contenedor Docker con Google Chrome basado en LinuxServer. Proporciona una interfaz web para usar Chrome en un navegador headless.

## Características

- 🌐 **Interfaz Web**: Acceso a Chrome vía navegador en puerto 3000
- 🔒 **Aislado**: Chrome ejecutándose en contenedor seguro
- ⚙️ **Configurable**: Variables de entorno para TZ, dominio, usuario y contraseña
- 🗣️ **Idioma Español**: Configurado con locale es_ES.UTF-8
- 💾 **Memoria Compartida**: shm_size de 1GB para estabilidad

## Requisitos Previos

- Docker Engine instalado
- Docker Compose instalado
- **Para Traefik o NPM**: Red Docker `proxy` creada
- **Dominio configurado**: Para acceso HTTPS

## Archivos de este Repositorio

Este repositorio contiene archivos de ejemplo:
- `docker-compose.yml` - Configuración base del contenedor
- `.env.example` - Plantilla de variables de entorno
- `docker-compose.override.traefik.yml.example` - Labels para Traefik
- `README.md` - Esta documentación

> 💡 **Tip**: Puedes copiar estos archivos manualmente o clonar el repositorio.

---

## Despliegue con Docker Compose

### 1. Crear Directorio y Archivos

```bash
# Crear directorio
mkdir chrome
cd chrome
```

### 2. Crear docker-compose.yml

Crea el archivo `docker-compose.yml`:

```yaml
services:
  chrome:
    image: lscr.io/linuxserver/chrome:latest
    container_name: chrome
    restart: unless-stopped
    #ports:
    #  - 3000:3000
    #  - 3001:3001
    volumes:
      - chrome_config:/config
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=${TZ:-Europe/Madrid}
      - LC_ALL=${LC_ALL:-es_ES.UTF-8}
      - CUSTOM_USER=${CUSTOM_USER:-admin}
      - PASSWORD=${PASSWORD:-password}
      - CHROME_CLI=${CHROME_CLI:-https://www.linuxserver.io}
    shm_size: "1gb"

volumes:
  chrome_config:

networks:
  default:
    external: true
    name: proxy
```

### 3. Configurar Variables de Entorno

Crea el archivo `.env`:

```env
# Zona horaria
TZ=Europe/Madrid

# Dominio para Traefik
DOMAIN_HOST=chrome.tudominio.com

# Usuario y contraseña
CUSTOM_USER=admin
PASSWORD=tu_contraseña_segura

# Idioma español de España
LC_ALL=es_ES.UTF-8

# Página inicial de Chrome
CHROME_CLI=https://www.google.es
```

### 4. (Opcional) Configurar Traefik

Si usas Traefik, crea `docker-compose.override.yml`:

```yaml
services:
  chrome:
    labels:
      - traefik.enable=true
      - traefik.http.routers.chrome.rule=Host(`${DOMAIN_HOST}`)
      - traefik.http.routers.chrome.entrypoints=websecure
      - traefik.http.routers.chrome.tls.certresolver=letsencrypt
      - traefik.http.services.chrome.loadbalancer.server.port=3000
```

### 5. Desplegar

```bash
# Crear red proxy si no existe
docker network create proxy

# Iniciar servicios
docker compose up -d

# Ver logs
docker compose logs -f chrome
```

---

## Método Alternativo: Clonar desde Git

Si prefieres usar Git para mantener la configuración actualizada:

```bash
# Clonar repositorio
git clone https://git.ictiberia.com/groales/chrome.git
cd chrome

# Copiar y editar variables
cp .env.example .env
nano .env

# Para Traefik
cp docker-compose.override.traefik.yml.example docker-compose.override.yml

# Desplegar
docker network create proxy
docker compose up -d
```

---

## Configuración con Proxy Inverso

**Nota**: Los puertos 3000 y 3001 están comentados en `docker-compose.yml` por defecto. Se recomienda usar un proxy inverso (Traefik o NPM) para acceso seguro. Si necesitas acceso directo, descomenta las líneas de `ports`.

### Traefik

1. Copia `docker-compose.override.traefik.yml.example` a `docker-compose.override.yml`
2. Edita `DOMAIN_HOST` en tu `.env` con tu dominio real
3. Asegúrate de que la red `proxy` existe
4. Deploy

### NPM (Nginx Proxy Manager)

No requiere override adicional. Solo configura un Proxy Host en NPM apuntando a `chrome:3000` en la red `proxy`.

## Acceso

- **Interfaz Web**: `https://${DOMAIN_HOST}` (configurado en Traefik)

## Volúmenes

- `chrome_config`: Configuración persistente de Chrome

## Recursos Oficiales

- [Documentación LinuxServer Chrome](https://docs.linuxserver.io/images/docker-chrome/)
- [GitHub LinuxServer](https://github.com/linuxserver/docker-chrome)