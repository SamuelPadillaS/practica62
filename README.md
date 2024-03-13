# Práctica: Creación de un sitio web estático con MkDocs y GitHub Pages
 Crearemos un repositorio con la sigueinte estrucutura:

~~~
.
├── .github
│   └── workflows
│   │   └── build-push-mkdocs.yaml
└── proyecto
    ├── docs
    │   └── index.md
    └── mkdocs.yml
~~~

Ya sabiendo la estructura informaremos de cada uno de los archivos y su contenido

### .github/workflows/build-push-mkdocs.yaml

~~~
name: build-push-mkdocs

# Eventos que desescandenan el workflow
on:
  push:
    branches: ["main"]

  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:

  # Job para crear la documentación de mkdocs
  build:
    # Indicamos que este job se ejecutará en una máquina virtual con la última versión de ubuntu
    runs-on: ubuntu-latest
    
    # Definimos los pasos de este job
    steps:
      - name: Clone repository
        uses: actions/checkout@v4

      - name: Install Python3
        uses: actions/setup-python@v4
        with:
          python-version: 3.x

      - name: Install Mkdocs
        run: |
          pip install mkdocs
          pip install mkdocs-material 

      - name: Build MkDocs
        run: |
          mkdocs build

      - name: Push the documentation in a branch
        uses: s0/git-publish-subdir-action@develop
        env:
          REPO: self
          BRANCH: gh-pages # The branch name where you want to push the assets
          FOLDER: site # The directory where your assets are generated
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # GitHub will automatically add this - you don't need to bother getting a token
          MESSAGE: "Build: ({sha}) {msg}" # The commit message
~~~

### docs

Aqui ira todo lo relacionado al contenido dentro de nuestras publicaciones

![Screenshot 2024-03-13 014228](https://github.com/SamuelPadillaS/practica62/assets/114667075/5ef10c38-5e74-4bfb-8fdd-340477a90caa)

### site
Aqui esta todo los archivos que hacen funcionar nuestra pagina web estatica

![Screenshot 2024-03-13 014301](https://github.com/SamuelPadillaS/practica62/assets/114667075/43a27bb4-f479-495e-9d7f-f80b16ea07e0)

Tamien importante añadirle permisosd e lectura y escritura a nuestro **WORKFLOW**

![descarga](https://github.com/SamuelPadillaS/practica62/assets/114667075/9999f8be-7b66-47e6-a40c-41443187ba37)

## Crear un servidor de desarrollo local (Comando: serve)

Una vez que hemos creado la estructura del proyecto podemos empezar el desarrollo de nuestro sitio iniciando un contenedor Docker con MkDocs y el theme Material.

~~~
docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material
~~~

### Generar la documentación (Comando: build)

~~~
docker run --rm -it -v "$PWD":/docs squidfunk/mkdocs-material build
~~~

### Publicar la documentación en GitHub Pages (Comando: gh-deploy)

~~~
docker run --rm -it -v ~/.ssh:/root/.ssh -v "$PWD":/docs squidfunk/mkdocs-material gh-deploy
~~~




