# Chrome

Contenedor Docker con Google Chrome basado en LinuxServer. Proporciona una interfaz web y acceso VNC para usar Chrome en un navegador headless.

## Características

- 🌐 **Interfaz Web**: Acceso a Chrome vía navegador en puerto 3000
- 🖥️ **VNC**: Acceso remoto vía VNC en puerto 3001
- 🔒 **Aislado**: Chrome ejecutándose en contenedor seguro
- ⚙️ **Configurable**: Variables de entorno para PUID/PGID/TZ

## Requisitos Previos

- Docker Engine instalado
- Portainer configurado (recomendado)
- **Para Traefik o NPM**: Red Docker `proxy` creada
- **Dominio configurado**: Para acceso HTTPS

## Variables de Entorno

Configura estas variables en un archivo `.env`:

```env
PUID=1000          # ID del usuario
PGID=1000          # ID del grupo
TZ=Europe/Madrid   # Zona horaria
```

## Despliegue con Portainer

### Opción A: Git Repository (Recomendada)

1. En Portainer, ve a **Stacks** → **Add stack**
2. Nombra el stack: `chrome`
3. URL del repo: `https://github.com/groales/chrome` (o `https://git.ictiberia.com/groales/chrome`)
4. Compose path: `docker-compose.yml`
5. Añade variables de entorno desde `.env`
6. **Deploy**

### Opción B: Web Editor

1. Copia el contenido de `docker-compose.yml`
2. Pega en **Stacks** → **Add stack** → **Web editor**
3. Añade las variables de entorno
4. **Deploy**

## Configuración con Proxy Inverso

### Traefik

1. Copia `docker-compose.override.traefik.yml.example` a `docker-compose.override.yml`
2. Edita los dominios:
   - `chrome.tudominio.com` para la interfaz web
   - `chrome-vnc.tudominio.com` para VNC
3. Asegúrate de que la red `proxy` existe
4. Deploy

### NPM (Nginx Proxy Manager)

1. Copia `docker-compose.override.npm.yml.example` a `docker-compose.override.yml`
2. En NPM, crea Proxy Hosts:
   - **Web UI**: `chrome.tudominio.com` → `chrome:3000`
   - **VNC**: `chrome-vnc.tudominio.com` → `chrome:3001`
3. Asegúrate de que la red `npm` existe
4. Deploy

## Acceso

- **Interfaz Web**: `http://localhost:3000` o `https://chrome.tudominio.com`
- **VNC**: `localhost:3001` con cliente VNC (usuario: abc, password: abc)

## Volúmenes

- `chrome_config`: Configuración persistente de Chrome

## Recursos Oficiales

- [Documentación LinuxServer Chrome](https://docs.linuxserver.io/images/docker-chrome/)
- [GitHub LinuxServer](https://github.com/linuxserver/docker-chrome)