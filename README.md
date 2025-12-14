# Chrome

Contenedor Docker con Google Chrome basado en LinuxServer. Proporciona una interfaz web para usar Chrome en un navegador headless.

## Características

- 🌐 **Interfaz Web**: Acceso a Chrome vía navegador en puerto 3000
- 🔒 **Aislado**: Chrome ejecutándose en contenedor seguro
- ⚙️ **Configurable**: Variables de entorno para TZ y dominio

## Requisitos Previos

- Docker Engine instalado
- Portainer configurado (recomendado)
- **Para Traefik**: Red Docker `proxy` creada
- **Dominio configurado**: Para acceso HTTPS

## Variables de Entorno

Configura estas variables en un archivo `.env`:

```env
TZ=Europe/Madrid       # Zona horaria
DOMAIN_HOST=tudominio.com  # Dominio para Traefik
CUSTOM_USER=usuario    # Usuario opcional para acceso
PASSWORD=contraseña_segura  # Contraseña opcional para acceso
LANG=es_ES.UTF-8       # Idioma español de España
```

## Despliegue con Portainer

### Opción A: Git Repository (Recomendada)

1. En Portainer, ve a **Stacks** → **Add stack**
2. Nombra el stack: `chrome`
3. URL del repo: `https://github.com/groales/chrome` (o `https://git.ictiberia.com/groales/chrome`)
4. Compose path: `docker-compose.yml`
5. **Solo para Traefik**: En **Additional paths**, añade:
   - `docker-compose.override.traefik.yml.example`
6. Añade variables de entorno desde `.env`
7. **Deploy**

### Opción B: Web Editor

1. Copia el contenido de `docker-compose.yml`
2. Pega en **Stacks** → **Add stack** → **Web editor**
3. Añade las variables de entorno
4. **Deploy**

## Configuración con Proxy Inverso

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