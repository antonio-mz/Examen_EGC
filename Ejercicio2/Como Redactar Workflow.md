### Referencia Completa: Eventos y Jobs en GitHub Actions

---

### **Índice**

1. [Resumen de Eventos en GitHub Actions](#resumen-de-eventos-en-github-actions)
2. [Eventos más comunes](#eventos-más-comunes)
   - [push](#1-push)
   - [pull_request](#2-pull_request)
   - [workflow_run](#3-workflow_run)
   - [workflow_dispatch](#4-workflow_dispatch)
   - [schedule](#5-schedule)
   - [issue_comment](#6-issue_comment)
3. [Sección `on`](#sección-on)
4. [Sección `jobs`](#sección-jobs)
   - [Estructura General](#estructura-general)
   - [Ejemplo con servicios y matrix](#ejemplo-1-job-con-servicios-y-matrix)
   - [Ejemplo de dependencia entre jobs](#ejemplo-2-job-dependiente-con-needs)
   - [Ejemplo de configuración básica](#ejemplo-3-configuración-básica-de-un-job)
5. [Resumen de secciones](#resumen-de-secciones)

---

### **Resumen de Eventos en GitHub Actions**
Un **evento** en GitHub Actions es una acción o suceso que inicia la ejecución de un workflow. Los eventos pueden ser automáticos (como un push) o manuales (como un disparador directo).

---

### **Eventos más comunes**

#### **Resumen de Usos Comunes**
| **Evento**          | **Cuándo se usa**                                    |
|----------------------|-----------------------------------------------------|
| `push`              | Validar cambios tras cada commit/push.              |
| `pull_request`      | Validar cambios en una PR antes de fusionarla.       |
| `workflow_run`      | Encadenar workflows dependientes.                   |
| `workflow_dispatch` | Disparar workflows manualmente desde GitHub.        |
| `schedule`          | Ejecutar tareas automáticas a intervalos regulares. |
| `issue_comment`     | Responder o actuar en base a comentarios en issues. |

Estos eventos permiten construir automatizaciones flexibles para diferentes necesidades en un proyecto.

---

### **1. `push`**
- **Descripción:** Se activa cuando se realiza un push a una rama del repositorio.
- **Ejemplo:**
  ```yaml
  on:
    push:
      branches:
        - main
        - develop
  ```
  **Significado:** El workflow se ejecutará cada vez que se haga un push a las ramas `main` o `develop`.

- **Usos comunes:**
  - Ejecutar pruebas automáticas.
  - Desplegar código después de un commit.

---

### **2. `pull_request`**
- **Descripción:** Se activa cuando se abre, actualiza o cierra una pull request.
- **Ejemplo:**
  ```yaml
  on:
    pull_request:
      branches:
        - main
  ```
  **Significado:** El workflow se ejecutará para pull requests dirigidas a la rama `main`.

- **Usos comunes:**
  - Validar cambios antes de fusionarlos en la rama principal.
  - Ejecutar pruebas automáticas en los cambios propuestos.

---

### **3. `workflow_run`**
- **Descripción:** Se activa cuando otro workflow se completa.
- **Ejemplo:**
  ```yaml
  on:
    workflow_run:
      workflows:
        - "Build and Test"
      types:
        - completed
  ```
  **Significado:** Este workflow se ejecutará cuando el workflow llamado `Build and Test` termine.

- **Usos comunes:**
  - Ejecutar despliegues solo si las pruebas del workflow anterior pasan.
  - Separar tareas complejas en workflows independientes.

---

### **4. `workflow_dispatch`**
- **Descripción:** Permite ejecutar workflows manualmente desde GitHub.
- **Ejemplo:**
  ```yaml
  on:
    workflow_dispatch:
      inputs:
        environment:
          description: 'Deploy environment'
          required: true
          default: 'production'
  ```
  **Significado:** El workflow se puede disparar manualmente desde la interfaz de GitHub, solicitando un entorno como entrada (`production` o `staging`).

- **Usos comunes:**
  - Desplegar aplicaciones manualmente.
  - Ejecutar tareas de mantenimiento.

---

### **5. `schedule`**
- **Descripción:** Permite ejecutar workflows automáticamente en horarios definidos usando expresiones CRON.
- **Ejemplo:**
  ```yaml
  on:
    schedule:
      - cron: '0 0 * * *'
  ```
  **Significado:** El workflow se ejecutará todos los días a medianoche (UTC).

- **Usos comunes:**
  - Ejecutar backups periódicos.
  - Tareas de mantenimiento recurrentes.

---

### **6. `issue_comment`**
- **Descripción:** Se activa cuando alguien comenta en un issue o pull request.
- **Ejemplo:**
  ```yaml
  on:
    issue_comment:
      types:
        - created
  ```
  **Significado:** El workflow se ejecutará cuando se cree un nuevo comentario en un issue o pull request.

- **Usos comunes:**
  - Responder automáticamente a comentarios específicos.
  - Marcar issues como resueltos en base a comentarios.

---

### **Sección `on`**
La sección `on` define los eventos que disparan la ejecución del workflow.

#### **Ejemplo 1: Eventos comunes**
```yaml
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
```
- **`push`**: Ejecuta el workflow cuando se realiza un push a la rama `main`.
- **`pull_request`**: Ejecuta el workflow cuando se crea o actualiza una pull request hacia `main`.

#### **Ejemplo 2: Despliegue con tags**
```yaml
on:
  push:
    tags:
      - 'v*'
  pull_request:
    branches:
      - main
```
- **`push` con `tags`**: Ejecuta el workflow solo cuando el push incluye un tag que sigue el patrón `v*` (ej., `v1.0.0`).
- **`pull_request`**: Ejecuta el workflow para pull requests hacia la rama `main`.

---

### **Sección `jobs`**
La sección `jobs` define las tareas a realizar dentro del workflow. Cada job es independiente y puede incluir pasos (“steps”), servicios y estrategias de ejecución.

#### **Estructura General**
```yaml
jobs:
  nombre_job:
    runs-on: ubuntu-latest
    steps:
      - name: Nombre del Paso
        run: comando
```
- **`nombre_job`**: Identificador del job (ej., `build`, `testing`, `deploy`).
- **`runs-on`**: Especifica el sistema operativo del runner.
- **`steps`**: Lista de pasos a ejecutar dentro del job.

---

### **Ejemplo 1: Job con servicios y matrix**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: uvlhub_root_password
          MYSQL_DATABASE: uvlhubdb_test
          MYSQL_USER: uvlhub_user
          MYSQL_PASSWORD: uvlhub_password
        ports:
          - 3306:3306
        options: --health-cmd="mysqladmin ping" --health-interval=10s --health-timeout=5s --health-retries=3

    strategy:
      matrix:
        python-version: ['3.11', '3.12']

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run Tests
        run: pytest
```
**Explicación:**
- **`services`**: Configura un contenedor MySQL como servicio para este job.
- **`strategy.matrix`**: Ejecuta el job en Python 3.11 y 3.12.
- **`steps`**: Realiza tareas como checkout del código, configurar Python, instalar dependencias y ejecutar pruebas.

---

### **Ejemplo 2: Job dependiente con `needs`**
```yaml
jobs:
  testing:
    name: Run Tests
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run Tests
        run: pytest

  deploy:
    name: Deploy to Render
    needs: testing
    runs-on: ubuntu-latest

    steps:
      - name: Check out the repo
        uses: actions/checkout@v4

      - name: Deploy to Render
        env:
          deploy_url: ${{ secrets.RENDER_DEPLOY_HOOK_URL }}
        run: |
          curl "$deploy_url"
```
**Explicación:**
- **`needs: testing`**: El job `deploy` depende del job `testing` y se ejecuta solo si este termina correctamente.
- **Pasos del `testing`**: Incluye checkout, configuración de Python, instalación de dependencias y ejecución de pruebas.
- **Pasos del `deploy`**: Incluye despliegue mediante un webhook con una URL almacenada en secretos.

---

### **Ejemplo 3: Configuración básica de un job**
```yaml
jobs:
  basic:
    runs-on: ubuntu-latest
    steps:
      - name: Hello World
        run: echo "Hello, world!"

      - name: Create a file
        run: echo "This is a test file." > test.txt
```
**Explicación:**
- Ejecuta comandos simples como imprimir mensajes y crear un archivo.
- Ideal para ejemplos básicos o pruebas iniciales.

---

### Resumen de Secciones

| Sección     | Propósito                                                                 |
|--------------|--------------------------------------------------------------------------|
| `on`         | Define los eventos que disparan la ejecución del workflow.              |
| `jobs`       | Contiene los trabajos a realizar y su configuración (e.g., `steps`).    |
| `steps`      | Define las acciones/tareas individuales dentro de un job.               |
| `services`   | Configura contenedores adicionales necesarios para los jobs.            |
| `strategy`   | Permite definir matrices de ejecución para probar diferentes entornos.   |
| `needs`      | Especifica dependencias entre jobs.                                      |

Este documento sirve como referencia rápida para estructurar workflows en GitHub Actions con `on` y `jobs`.

