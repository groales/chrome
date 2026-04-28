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
- **Dominio configurado**: Para acceso HTTPS

## Archivos de este Repositorio

Este repositorio contiene archivos de ejemplo:
- `compose.yaml` - Configuración base del contenedor
- `.env.example` - Plantilla de variables de entorno
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

### 2. Crear compose.yaml

Crea el archivo `compose.yaml`:

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
      - ./config:/config
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=${TZ:-Europe/Madrid}
      - LC_ALL=${LC_ALL:-es_ES.UTF-8}
      - CUSTOM_USER=${CUSTOM_USER:-admin}
      - PASSWORD=${PASSWORD:-password}
      - CHROME_CLI=${CHROME_CLI:-https://www.linuxserver.io}
    shm_size: "1gb"

# añadir estas líneas al final del archivo para proxy inverso 
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

DOMAIN_HOST=chrome.tudominio.com

# Usuario y contraseña
CUSTOM_USER=admin
PASSWORD=tu_contraseña_segura

# Idioma español de España
LC_ALL=es_ES.UTF-8

# Página inicial de Chrome
CHROME_CLI=https://www.google.es
```



```yaml
services:
  chrome:
    labels:
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


# Desplegar
docker network create proxy
docker compose up -d
```

---

## Configuración con Proxy Inverso



2. Edita `DOMAIN_HOST` en tu `.env` con tu dominio real
3. Asegúrate de que la red `proxy` existe
4. Deploy



## Acceso


## Volúmenes

- `chrome_config`: Configuración persistente de Chrome

## Recursos Oficiales

- [Documentación LinuxServer Chrome](https://docs.linuxserver.io/images/docker-chrome/)
- [GitHub LinuxServer](https://github.com/linuxserver/docker-chrome)
