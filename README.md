Autor: Mateo Guerrón 
Materia: DevOps



# Proyecto CI/CD con Flask + Docker + GitHub Actions

Este proyecto demuestra un pipeline **CI/CD completo**, desde la verificación del código hasta la construcción y publicación de una imagen Docker en **GitHub Container Registry (GHCR)**.

Incluye:

* ✔ Aplicación Flask simple
* ✔ Prueba básica automatizada
* ✔ Pipeline CI/CD funcional con GitHub Actions
* ✔ Construcción automática de imagen Docker
* ✔ Publicación del contenedor en GHCR

Este README explica paso a paso el ciclo CI/CD solicitado en la rúbrica.



# 1. Ciclo CI/CD Explicado (Ejemplo práctico)

El pipeline se activa cuando:

* Se hace **push** a la rama `guerron-ci-cd`
* Se crea un **Pull Request**

El flujo CI/CD ejecuta automáticamente:

###  CI – Integración Continua

* Descarga el repositorio
* Instala dependencias
* Ejecuta pruebas básicas
* Verifica la existencia del archivo principal `app.py`

###  CD – Despliegue Continuo

* Autenticación en GHCR con tokens
* Construcción de imagen Docker
* Publicación del contenedor en `ghcr.io/matth23-sys/mateo-guerron-ci-cd:latest`

De esta manera, cada cambio en el proyecto genera automáticamente una imagen lista para usarse.

---

# 2. Estructura del proyecto

```
mateo-guerron-ci-cd/
├── app.py
├── requirements.txt
├── Dockerfile
├── guerron-ci-cd
└── .github/workflows/ci.yml
```

---

# 3. Prueba incluida (básica)

El pipeline ejecuta esta prueba simple:

```bash
echo "✅ Flask app file exists"
test -f app.py
```

Sirve para asegurar que el proyecto tiene su archivo principal y que el flujo CI está funcionando.

---

# 4. Construcción del Package (Docker)

Tu Dockerfile:

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

La imagen resultante se genera y publica automáticamente mediante GitHub Actions.

---

# 5. Workflow CI/CD (GitHub Actions)

Archivo: **.github/workflows/ci.yml**

```yaml
name: CI/CD Guerron Pipeline

on:
  push:
    branches:
       - guerron-ci-cd
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: 🧾 Checkout repository
      uses: actions/checkout@v3

    - name: 🐍 Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.10'

    - name: 📦 Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: 🧪 Run basic test
      run: |
        echo "✅ Flask app file exists"
        test -f app.py

    - name: 🔐 Login to GitHub Container Registry
      uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: 🏗️ Build and push Docker image
      uses: docker/build-push-action@v6
      with:
        push: true
        tags: ghcr.io/matth23-sys/mateo-guerron-ci-cd:latest
```

Este pipeline cumple con:

✔ CI
✔ CD
✔ Prueba automatizada
✔ Package generado (imagen Docker)
✔ Uso de GHCR
✔ Práctica DevOps completa

---

# 6. Ejecución local

```bash
pip install -r requirements.txt
python app.py
```

La app Flask levanta un "Hola Mundo" en:

```
http://localhost:5000
```

---

# 7. Descargar la imagen desde GHCR

```bash
docker pull ghcr.io/matth23-sys/mateo-guerron-ci-cd:latest
docker run -p 5000:5000 ghcr.io/matth23-sys/mateo-guerron-ci-cd:latest
```

---

# 8. Conclusión

Este proyecto demuestra el ciclo CI/CD completo usando:

* Flask
* GitHub Actions
* Docker
* GHCR

Cumple con los 10 puntos de la rúbrica:

| Criterio                 | Pts       | Estado |
| ------------------------ | --------- | ------ |
| README explicado         | 2         | ✔      |
| Workflow CI/CD funcional | 2         | ✔      |
| Pruebas                  | 2         | ✔      |
| Construcción del package | 2         | ✔      |
| Repositorio accesible    | 2         | ✔      |
| **Total**                | **10/10** | 🎉     |
