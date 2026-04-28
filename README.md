# Chrome

Contenedor Docker con Google Chrome basado en LinuxServer, accesible por navegador (KasmVNC).

Referencia oficial de instalación: https://docs.linuxserver.io/images/docker-chrome/

## Características

- Interfaz web para navegador remoto.
- Configurable por variables de entorno.
- Persistencia local en `./config`.
- Soporte de acceso directo opcional por puertos `3000` y `3001`.

## Requisitos Previos

- Docker Engine instalado.
- Docker Compose instalado.
- Red Docker externa `proxy` creada si lo publicarás detrás de proxy inverso.

## Archivos de este Repositorio

- `compose.yaml` - Definición del servicio Chrome.
- `.env.example` - Variables de entorno recomendadas.
- `README.md` - Esta documentación.

---

## Despliegue con Docker Compose

### 1. Clonar el repositorio

```bash
git clone https://github.com/groales/chrome.git
cd chrome
```

### 2. Preparar variables de entorno

```bash
cp .env.example .env
```

### 3. Revisar `compose.yaml`

```yaml
services:
  chrome:
    image: lscr.io/linuxserver/chrome:latest
    container_name: chrome
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=${TZ:-Europe/Madrid}
      - LC_ALL=${LC_ALL:-es_ES.UTF-8}
      - CUSTOM_USER=${CUSTOM_USER:-admin}
      - PASSWORD=${PASSWORD:-password}
      - CHROME_CLI=${CHROME_CLI:-https://www.linuxserver.io}
    volumes:
      - ./config:/config
    #Puertos expuestos para acceso directo (opcional, se recomienda usar proxy inverso)
    #ports:
    #  - 3000:3000
    #  - 3001:3001
    shm_size: "1gb"
    restart: unless-stopped

networks:
  default:
    external: true
    name: proxy
```

### 4. Levantar el servicio

```bash
docker network create proxy
docker compose up -d
```

---

## Método Alternativo: Crear Manualmente

Puedes copiar `compose.yaml` y `.env.example` manualmente en una carpeta nueva y ejecutar los mismos comandos de despliegue.

---

## Acceso Inicial

- Con proxy inverso: usa tu dominio configurado.
- Acceso directo opcional: descomenta los puertos y accede a `http://IP_SERVIDOR:3000`.

## Comandos Útiles

```bash
docker compose logs -f chrome
docker compose restart chrome
docker compose pull
docker compose up -d
docker compose down
```

## Estructura de Volúmenes

```text
Bind mount:
└── ./config -> /config
```

## Configuración Avanzada

- `CHROME_CLI` define la URL inicial al abrir Chrome.
- `LC_ALL` permite forzar idioma/región.
- `CUSTOM_USER` y `PASSWORD` controlan credenciales de acceso.

## Solución de Problemas

Si no abre por dominio:

- Verifica DNS.
- Verifica que el proxy enruta al contenedor `chrome`.
- Revisa logs con `docker compose logs -f chrome`.

## Seguridad

- Usa HTTPS para acceso remoto.
- Evita exponer puertos directos a Internet.
- Usa contraseña robusta en `PASSWORD`.

## Backup y Restauración

```bash
# Backup
tar -czf chrome-config-$(date +%Y%m%d).tar.gz ./config

# Restauración
docker compose down
rm -rf ./config
tar -xzf chrome-config-YYYYMMDD.tar.gz
docker compose up -d
```

## Actualización

```bash
docker compose pull
docker compose up -d
docker compose logs -f chrome
```

## Recursos

- LinuxServer Chrome Docs: https://docs.linuxserver.io/images/docker-chrome/
- Repositorio LinuxServer: https://github.com/linuxserver/docker-chrome

## Licencia

Este repositorio de configuración es de uso libre. Revisa la licencia del proyecto original en su repositorio oficial.
