# INYECCION-SQL-en-DVWA-con-DOCKER
— Instalar DVWA en Docker

> **DVWA (Damn Vulnerable Web Application)** es una aplicación web intencionalmente vulnerable para practicar pruebas de seguridad web (SQLi, XSS, CSRF, RCE, etc.). Úsala **solo en entornos de laboratorio**.

---

## 1) Requisitos previos

- CachyOS:
  ```bash
  sudo pacman -Syu
  ```
- Docker y Compose:
  ```bash
  sudo pacman -S docker docker-compose
  ```
  > En Docker moderno se usa `docker compose`. Si el subcomando no aparece, usa `docker-compose` (con guion) y/o verifica que el paquete esté instalado.

- Habilitar y arrancar Docker, y permitir usarlo sin `sudo`:
  ```bash
  sudo systemctl enable --now docker
  sudo usermod -aG docker $USER
  # Aplica el grupo sin cerrar sesión (opcional):
  newgrp docker
  ```

- Probar Docker:
  ```bash
  docker run --rm hello-world
  ```

> **Nota de firewall/puertos:** Expondremos DVWA en el puerto `80`. Si ya está en uso, cambia el mapeo (por ejemplo `:82`).

---

## 2) Opción: 1 contenedor

Usaremos la imagen `vulnerables/web-dvwa`, que incluye Apache/PHP y MariaDB en el mismo contenedor.

1. Descargar la imagen:
   ```bash
   docker pull vulnerables/web-dvwa:latest
   ```

2. Crear un volumen para persistir la base de datos (opcional pero recomendado):
   ```bash
   docker volume create dvwa_mysql
   ```

3. Levantar DVWA mapeando el puerto 8080:
   ```bash
   docker run -d --name dvwa \
     -p 8080:80 \
     -v dvwa_mysql:/var/lib/mysql \
     --restart unless-stopped \
     vulnerables/web-dvwa:latest
   ```

4. Abrir en el navegador:
   - URL: <http://localhost:8080>
   - Usuario: `admin`
   - Clave: `password`

5. Dentro de DVWA (panel izquierdo → **DVWA Security** / **Setup**):
   - Ir a **Setup** y presionar **Create / Reset Database** la primera vez.
   - En **DVWA Security**, puedes elegir el nivel **low / medium / high / impossible** según lo que necesites practicar.
