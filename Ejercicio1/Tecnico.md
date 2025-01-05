### Guía Paso a Paso: Ejercicios Avanzados de Git

---

### **Ejercicio 1: Integrar cambios específicos con Cherry-Pick**

#### **1. Crear una rama `ch1` y realizar 3 commits**
1. Cambia a la rama principal:
   ```bash
   git checkout main
   ```
2. Crea la nueva rama `ch1`:
   ```bash
   git branch ch1
   git checkout ch1
   ```
3. Realiza tres cambios en los archivos de tu preferencia y haz un commit por cada cambio:
   ```bash
   git add .
   git commit -m "Primer cambio en ch1"
   git add .
   git commit -m "Segundo cambio en ch1"
   git add .
   git commit -m "Tercer cambio en ch1"
   ```
📷 **Captura sugerida**: Pantalla mostrando los 3 commits realizados en la rama `ch1`.

#### **2. Integrar únicamente el segundo commit en `egc_test` con Cherry-Pick**
1. Cambia a la rama `egc_test`:
   ```bash
   git checkout egc_test
   ```
2. Encuentra el ID del segundo commit en `ch1`:
   ```bash
   git log ch1
   ```
3. Aplica el segundo commit en `egc_test` usando Cherry-Pick:
   ```bash
   git cherry-pick <id_del_segundo_commit>
   ```
📷 **Captura sugerida**: Pantalla mostrando el commit aplicado en `egc_test`.

---

### **Ejercicio 2: Rebase interactivo para combinar commits**

#### **1. Crear una rama `rbs` y realizar 5 commits**
1. Cambia a la rama principal:
   ```bash
   git checkout main
   ```
2. Crea la nueva rama `rbs`:
   ```bash
   git branch rbs
   git checkout rbs
   ```
3. Realiza cinco cambios y haz un commit por cada cambio:
   ```bash
   git add .
   git commit -m "Commit a"
   git add .
   git commit -m "Commit b"
   git add .
   git commit -m "Commit c"
   git add .
   git commit -m "Commit d"
   git add .
   git commit -m "Commit e"
   ```
📷 **Captura sugerida**: Pantalla mostrando los 5 commits realizados en la rama `rbs`.

#### **2. Combinar los commits `b`, `c` y `d` en uno solo**
1. Inicia un rebase interactivo:
   ```bash
   git rebase -i HEAD~5
   ```
2. En el editor de texto:
   - Cambia `pick` por `squash` para los commits `c` y `d`.
   - Deja el commit `b` como `pick`.
3. Guarda y cierra el editor.
4. Edita el mensaje combinado del commit si es necesario y guarda.
📷 **Capturas sugeridas**:
- Pantalla del editor mostrando los cambios en el rebase interactivo.
- Historial final con los commits `a`, `bcd` y `e`.

---

### **Ejercicio 3: Deshacer cambios con Reset Soft**

#### **1. Crear una rama `rv1` y realizar 3 commits**
1. Crea y cambia a la rama `rv1`:
   ```bash
   git branch rv1
   git checkout rv1
   ```
2. Realiza tres cambios en el archivo `visualizer.html` y haz un commit por cada cambio:
   ```bash
   git add .
   git commit -m "Primer cambio"
   git add .
   git commit -m "Segundo cambio"
   git add .
   git commit -m "Tercer cambio"
   ```
📷 **Captura sugerida**: Pantalla mostrando los 3 commits realizados en la rama `rv1`.

#### **2. Deshacer los dos últimos commits de manera Soft**
1. Usa reset soft para deshacer los dos últimos commits:
   ```bash
   git reset --soft HEAD~2
   ```
2. Verifica que los cambios siguen en el área de staging:
   ```bash
   git status
   ```
📷 **Captura sugerida**: Pantalla mostrando el estado después del reset soft.

---

### **Ejercicio 4: Resolver conflictos de fusiones**

#### **1. Crear dos ramas `rq1` y `rq2`**
1. Crea las ramas y realiza cambios conflictivos en ambas:
   ```bash
   git branch rq1
   git checkout rq1
   # Realiza cambios en /decide/census/models.py
   git add .
   git commit -m "Cambio conflictivo en rq1"

   git checkout main
   git branch rq2
   git checkout rq2
   # Realiza otro cambio conflictivo en /decide/census/models.py
   git add .
   git commit -m "Cambio conflictivo en rq2"
   ```
📷 **Captura sugerida**: Pantalla mostrando los commits realizados en ambas ramas.

#### **2. Fusionar y resolver conflictos**
1. Cambia a la rama `egc_test`:
   ```bash
   git checkout egc_test
   ```
2. Fusiona la rama `rq1`:
   ```bash
   git merge rq1
   ```
3. Fusiona la rama `rq2` y resuelve los conflictos:
   ```bash
   git merge rq2
   ```
   - Edita los archivos conflictivos para resolver el conflicto.
   - Marca el conflicto como resuelto:
     ```bash
     git add /decide/census/models.py
     ```
   - Finaliza el merge:
     ```bash
     git commit
     ```
📷 **Captura sugerida**: Pantalla mostrando la resolución del conflicto.

---

Con estas guías detalladas, puedes completar cada ejercicio con capturas que demuestran el progreso y los resultados.

