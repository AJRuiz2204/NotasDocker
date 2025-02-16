# NotasDocker
Primero, el error "Cannot connect to the Docker daemon" significa que el daemon de Docker no se está ejecutando o tu usuario no tiene permisos para acceder a él. Para resolverlo, sigue estos pasos:

1. **Verifica que el daemon de Docker esté activo:**  
   Ejecuta:
   ```bash
   sudo systemctl status docker
   ```
   Si no está activo, inícialo con:
   ```bash
   sudo systemctl start docker
   ```
   Y si quieres que se inicie automáticamente al arrancar la instancia, usa:
   ```bash
   sudo systemctl enable docker
   ```

2. **Permisos de usuario:**  
   Asegúrate de que tu usuario (por ejemplo, `ec2-user`) esté en el grupo `docker`:
   ```bash
   sudo usermod -aG docker ec2-user
   ```
   Luego, cierra la sesión y vuelve a conectarte para que los cambios surtan efecto, o utiliza `sudo` con los comandos de Docker.

---

### Uso de sysctl con Docker

Si además necesitas establecer parámetros del kernel para un contenedor, Docker permite usar la opción `--sysctl` en el comando `docker run` o especificarla en un archivo de Docker Compose.

**Ejemplo con `docker run`:**
```bash
docker run --sysctl net.ipv4.ip_forward=1 -d <imagen>
```

**Ejemplo en un archivo docker-compose.yml:**
```yaml
version: "3.9"
services:
  mi_servicio:
    image: mi_imagen
    sysctls:
      net.ipv4.ip_forward: 1
```

Estos parámetros permiten configurar variables del kernel para el contenedor.  
Pero recuerda, antes de usar `sysctl`, es necesario que el daemon de Docker esté funcionando correctamente.

---

Aquí tienes una lista de los comandos Docker y Docker Compose más comunes y útiles para gestionar contenedores, imágenes y otros recursos. Esta lista te servirá como referencia para las tareas diarias:

---

## **Comandos Básicos de Docker**

- **Listar contenedores en ejecución:**

  ```bash
  docker ps
  ```

- **Listar todos los contenedores (incluidos detenidos):**

  ```bash
  docker ps -a
  ```

- **Mostrar logs de un contenedor:**

  ```bash
  docker logs <nombre_o_ID_del_contenedor>
  ```

- **Ejecutar un comando dentro de un contenedor en ejecución:**

  ```bash
  docker exec -it <nombre_o_ID_del_contenedor> <comando>
  ```
  
  *Ejemplo:*  
  ```bash
  docker exec -it deploy-backend-1 bash
  ```

- **Adjuntar tu terminal a un contenedor (para ver la salida en tiempo real):**

  ```bash
  docker attach <nombre_o_ID_del_contenedor>
  ```

- **Iniciar un nuevo contenedor (modo interactivo):**

  ```bash
  docker run -it <imagen> bash
  ```
  
  *Ejemplo:*  
  ```bash
  docker run -it ubuntu bash
  ```

- **Detener un contenedor:**

  ```bash
  docker stop <nombre_o_ID_del_contenedor>
  ```

- **Reiniciar un contenedor:**

  ```bash
  docker restart <nombre_o_ID_del_contenedor>
  ```

- **Eliminar (remover) un contenedor detenido:**

  ```bash
  docker rm <nombre_o_ID_del_contenedor>
  ```

- **Eliminar una imagen:**

  ```bash
  docker rmi <nombre_o_ID_de_la_imagen>
  ```

- **Listar imágenes:**

  ```bash
  docker images
  ```

- **Construir una imagen a partir de un Dockerfile:**

  ```bash
  docker build -t <nombre_imagen>:<tag> .
  ```

- **Ver información detallada de un contenedor:**

  ```bash
  docker inspect <nombre_o_ID_del_contenedor>
  ```

---

## **Comandos de Docker Compose**

Si utilizas Docker Compose para orquestar varios contenedores, estos son los comandos más comunes:

- **Levantar los servicios definidos en el docker-compose.yml:**

  ```bash
  docker-compose up
  ```
  
  Para ejecutarlo en segundo plano (detached):
  
  ```bash
  docker-compose up -d
  ```

- **Construir o reconstruir los servicios (imágenes):**

  ```bash
  docker-compose build
  ```

  Para forzar la reconstrucción sin caché:

  ```bash
  docker-compose build --no-cache
  ```

- **Ver los logs de todos los servicios:**

  ```bash
  docker-compose logs
  ```
  
  Para ver en tiempo real:
  
  ```bash
  docker-compose logs -f
  ```

- **Detener los servicios:**

  ```bash
  docker-compose down
  ```

  Puedes agregar la opción `-v` para eliminar los volúmenes asociados:
  
  ```bash
  docker-compose down -v
  ```

- **Ver el estado de los servicios:**

  ```bash
  docker-compose ps
  ```

- **Ejecutar un comando en un servicio en ejecución:**

  ```bash
  docker-compose exec <nombre_del_servicio> <comando>
  ```
  
  *Ejemplo:*
  
  ```bash
  docker-compose exec sonrie-frontend bash
  ```

---

## **Consejos Adicionales**

- **Actualizar Docker y Docker Compose:**  
  Mantén tus versiones actualizadas para contar con las últimas funciones y mejoras de seguridad.

- **Utilizar `--help`:**  
  Cada comando admite la opción `--help` para ver más detalles y opciones disponibles. Por ejemplo:
  ```bash
  docker run --help
  docker-compose up --help
  ```

- **Gestión de Redes y Volúmenes:**  
  Puedes listar redes con `docker network ls` y volúmenes con `docker volume ls`. También, para inspeccionar una red o volumen:
  ```bash
  docker network inspect <nombre_de_la_red>
  docker volume inspect <nombre_del_volumen>
  ```

  Para detener, eliminar y levantar todos los servicios definidos en tu docker-compose, puedes usar los siguientes comandos:

1. **Detener y eliminar los contenedores y redes (sin borrar volúmenes):**

   ```bash
   docker-compose down
   ```

2. **Si también deseas eliminar los volúmenes (útil en desarrollo para empezar desde cero):**

   ```bash
   docker-compose down -v
   ```

3. **Levantar los servicios en segundo plano:**

   ```bash
   docker-compose up -d
   ```

4. **Si necesitas forzar la reconstrucción de las imágenes (por ejemplo, después de cambios en el Dockerfile):**

   ```bash
   docker-compose up --build -d
   ```

Estos comandos te permitirán detener y reiniciar todos los servicios de Docker definidos en tu archivo `docker-compose.yml`.
