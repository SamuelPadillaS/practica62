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

Aqui ira todo lo relacionado a los post que realicemos en nuestro blog



