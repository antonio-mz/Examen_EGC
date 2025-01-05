### Resumen: Eventos en GitHub Actions

Un **evento** en GitHub Actions es una acción o suceso que inicia la ejecución de un workflow. Los eventos pueden ser automáticos (como un push) o manuales (como un disparador directo). A continuación se explican los eventos más comunes:

---
### **Resumen de Usos Comunes**
| **Evento**          | **Cuándo se usa**                                    |
|----------------------|-----------------------------------------------------|
| `push`              | Validar cambios tras cada commit/push.              |
| `pull_request`      | Validar cambios en una PR antes de fusionarla.       |
| `workflow_run`      | Encadenar workflows dependientes.                   |
| `workflow_dispatch` | Disparar workflows manualmente desde GitHub.        |
| `schedule`          | Ejecutar tareas automáticas a intervalos regulares. |
| `issue_comment`     | Responder o actuar en base a comentarios en issues. |

Estos eventos permiten construir automatizaciones flexibles para diferentes necesidades en un proyecto.


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

### **Resumen de Usos Comunes**
| **Evento**          | **Cuándo se usa**                                    |
|----------------------|-----------------------------------------------------|
| `push`              | Validar cambios tras cada commit/push.              |
| `pull_request`      | Validar cambios en una PR antes de fusionarla.       |
| `workflow_run`      | Encadenar workflows dependientes.                   |
| `workflow_dispatch` | Disparar workflows manualmente desde GitHub.        |
| `schedule`          | Ejecutar tareas automáticas a intervalos regulares. |
| `issue_comment`     | Responder o actuar en base a comentarios en issues. |

Estos eventos permiten construir automatizaciones flexibles para diferentes necesidades en un proyecto.

