### Resumen: Configuración de Docker

---

#### **1. Configurar archivos de entorno**
Primero, copia el archivo `.env.docker.example` al archivo `.env` que se usará para establecer las variables de entorno.

```bash
cp .env.docker.example .env
```

---

#### **2. Ejecutar los contenedores**
Para iniciar los contenedores en modo desarrollo, usa el archivo `docker-compose.dev.yml` ubicado en el directorio `docker`. Este comando se ejecutará en segundo plano (`-d`).

```bash
docker compose -f docker/docker-compose.dev.yml up -d
```

---

#### **3. Verificar contenedores en ejecución**
Para verificar que los contenedores están corriendo correctamente, utiliza el siguiente comando:

```bash
docker ps
```

Si todo funciona correctamente, deberías ver la versión desplegada de **uvlhub** en desarrollo en: [http://localhost](http://localhost).

---

#### **4. Detener los contenedores**
Para detener los contenedores, usa el archivo `docker-compose.dev.yml` con el siguiente comando:

```bash
docker compose -f docker/docker-compose.dev.yml down
```

---

#### **5. Detener los contenedores (eliminando también los volúmenes)**
El comando anterior elimina los contenedores, pero no los volúmenes. Esto puede ser problemático en el caso de **MariaDB**, que sigue guardando la configuración anterior y puede generar problemas si queremos cargar una configuración diferente.

Para detener los contenedores y eliminar los volúmenes, utiliza la bandera `-v`:

```bash
docker compose -f docker/docker-compose.dev.yml down -v
```

---

#### **6. Recargar configuración**
Si se ha modificado algún archivo `Dockerfile` o `docker-compose.*.yml`, es necesario reconstruir las imágenes con la bandera `--build`. Para esto, ejecuta:

```bash
docker compose -f docker/docker-compose.dev.yml up -d --build
```

