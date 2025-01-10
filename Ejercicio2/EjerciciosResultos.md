# Modificaciones al Workflow `django.yml`

## **Intensificación colaborativa**

### Enunciado
1. Modifique el workflow `django.yml` para que utilice la versión de Python 3.11.  
2. Prepare el workflow para que la integración con Codacy constituya un nuevo job llamado `cobertura`.  
3. Haga commit y push de los cambios realizados.  
4. Verifique el correcto funcionamiento del workflow.

### Solución

#### Modificación de Python a 3.11:
```yaml
- name: Set up Python 3.11
  uses: actions/setup-python@v4
  with:
    python-version: 3.11
```

#### Separar Codacy en un nuevo job llamado `cobertura`:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    # Resto del job build permanece igual

  cobertura:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Codacy Coverage Reporter
        uses: codacy/codacy-coverage-reporter-action@v1
        with:
          project-token: ${{ secrets.CODACY_PROJECT_TOKEN }}
          coverage-reports: decide/coverage.xml
```

---

## **Balance técnico-organizativo**

### Enunciado
1. Configure el workflow `django.yml` para lanzar las pruebas con dos versiones de PostgreSQL diferentes (14.9 y 15).  
2. Haga commit y push de los cambios realizados.  
3. Verifique el correcto funcionamiento del workflow.

### Solución

#### Pruebas con matriz para PostgreSQL 14.9 y 15:
```yaml
strategy:
  matrix:
    postgres-version: [14.9, 15]

services:
  postgres:
    image: postgres:${{ matrix.postgres-version }}
    env:
      POSTGRES_USER: decide
      POSTGRES_PASSWORD: decide
      POSTGRES_DB: decide
    ports:
      - 5432:5432
    options: > 
      --health-cmd "pg_isready" 
      --health-interval 10s 
      --health-timeout 5s 
      --health-retries 5
```

#### Commit y Push:
```bash
git add .github/workflows/django.yml
git commit -m "Update django.yml for PostgreSQL matrix and Codacy job"
git push origin <branch>
```

---

## **Verificación**
1. Verifique que los jobs de `build` y `cobertura` se ejecuten correctamente en GitHub Actions.  
2. Asegúrese de que las pruebas se ejecuten con ambas versiones de PostgreSQL y reporten cobertura con éxito a Codacy.

--- 

Modifique el workflow django.yml para pasar las pruebas exclusivamente del módulo voting. 📷
Prepare el workflow para que se ejecute cuando haya cambios únicamente en master/main. 📷
Haga commit y push de los cambios realizados.
Verifique el correcto funcionamiento del workflow. 
- name: Run Tests
  env:
    FLASK_ENV: testing
    MARIADB_HOSTNAME: 127.0.0.1
    MARIADB_PORT: 3306
    MARIADB_TEST_DATABASE: uvlhubdb_test
    MARIADB_USER: uvlhub_user
    MARIADB_PASSWORD: uvlhub_password
  run: |
    pytest app/modules/nombreModulo\
      --ignore-glob='*selenium*'

  Si quiero ignorar otro tipo de pruebas
- name: Run Tests
  env:
    FLASK_ENV: testing
    MARIADB_HOSTNAME: 127.0.0.1
    MARIADB_PORT: 3306
    MARIADB_TEST_DATABASE: uvlhubdb_test
    MARIADB_USER: uvlhub_user
    MARIADB_PASSWORD: uvlhub_password
  run: |
    pytest app/modules/ \
      --ignore-glob='*selenium*' \
      --ignore-glob='*unit*'
**Notas:**  
Este diseño garantiza modularidad, pruebas en entornos diversos y una mejor integración continua con herramientas externas como Codacy.
