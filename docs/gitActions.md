# Automatización con GitHub Actions

En este apartado se documenta el proceso de creación e implementación del workflow de GitHub Actions que automatiza la generación y publicación de la documentación del proyecto.

---

## ¿Qué es GitHub Actions?

GitHub Actions es una plataforma de integración continua y entrega continua (CI/CD) que permite automatizar flujos de trabajo directamente desde el repositorio de GitHub. Permite compilar, probar y desplegar código de forma automática cuando ocurren ciertos eventos en el repositorio.

### Ventajas principales:
- **Automatización completa**: Se ejecuta sin intervención humana
- **Integración nativa**: Funciona directamente en GitHub
- **Gratuito para repositorios públicos**: Sin coste adicional
- **Altamente configurable**: Se adapta a cualquier necesidad
- **Ecosistema extenso**: Miles de actions predefinidas disponibles

---

## Estructura del Workflow

### Ubicación del archivo:
```
.github/workflows/CreacionDocumentacion.yml
```

### Justificación de la ubicación:
GitHub Actions busca automáticamente archivos de configuración en la carpeta `.github/workflows/`. Cualquier archivo `.yml` o `.yaml` en este directorio será interpretado como un workflow.

---

## Código del Workflow

```yaml
name: Generar documentación con MkDocs

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    permissions:
      contents: write
    
    steps:
      - name: Checkout del repositorio
        uses: actions/checkout@v4
      
      - name: Configurar Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      
      - name: Instalar dependencias
        run: |
          pip install mkdocs
          pip install mkdocs-material
      
      - name: Generar documentación
        run: mkdocs build
      
      - name: Publicar en GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

---

## Explicación detallada del Workflow

### 1. Nombre del workflow
```yaml
name: Generar documentación con MkDocs
```
**Justificación**: Define un nombre descriptivo que aparecerá en la pestaña "Actions" de GitHub, facilitando su identificación.

---

### 2. Eventos disparadores (Triggers)
```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:
```

**Justificación**:
- `push: branches: - main`: El workflow se ejecuta automáticamente cada vez que se realiza un push a la rama `main`
- `workflow_dispatch`: Permite ejecutar el workflow manualmente desde la interfaz de GitHub, útil para pruebas o regeneraciones bajo demanda

---

### 3. Definición del Job
```yaml
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
```

**Justificación**:
- `build-and-deploy`: Nombre descriptivo del trabajo que se va a ejecutar
- `runs-on: ubuntu-latest`: Especifica que el job se ejecutará en una máquina virtual con Ubuntu en su última versión estable. Ubuntu es ideal por ser ligero, rápido y compatible con la mayoría de herramientas de desarrollo

---

### 4. Permisos
```yaml
permissions:
  contents: write
```

**Justificación**: Otorga permisos de escritura al workflow para que pueda crear y modificar la rama `gh-pages` donde se publicará la documentación. Sin este permiso, el workflow fallaría al intentar hacer push.

---

### 5. Step 1: Checkout del repositorio
```yaml
- name: Checkout del repositorio
  uses: actions/checkout@v4
```

**Justificación**: Este step clona el repositorio en la máquina virtual del runner. Es necesario para acceder a todos los archivos del proyecto, incluida la configuración de MkDocs y los archivos Markdown.

---

### 6. Step 2: Configurar Python
```yaml
- name: Configurar Python
  uses: actions/setup-python@v4
  with:
    python-version: '3.x'
```

**Justificación**: Instala Python en la máquina virtual, necesario porque MkDocs es una herramienta escrita en Python. La versión `3.x` garantiza que se use la última versión estable de Python 3.

---

### 7. Step 3: Instalar dependencias
```yaml
- name: Instalar dependencias
  run: |
    pip install mkdocs
    pip install mkdocs-material
```

**Justificación**:
- Instala MkDocs, la herramienta que genera la documentación estática
- Instala el tema Material (opcional pero recomendado) que proporciona un diseño moderno y responsive
- El símbolo `|` permite ejecutar múltiples comandos en secuencia

---

### 8. Step 4: Generar documentación
```yaml
- name: Generar documentación
  run: mkdocs build
```

**Justificación**: Ejecuta el comando que lee todos los archivos Markdown del proyecto y genera los archivos HTML estáticos en la carpeta `site/`. Esta carpeta contendrá la web completa lista para ser servida.

---

### 9. Step 5: Publicar en GitHub Pages
```yaml
- name: Publicar en GitHub Pages
  uses: peaceiris/actions-gh-pages@v3
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    publish_dir: ./site
```

**Justificación**:
- Utiliza una acción predefinida especializada en publicar contenido en GitHub Pages
- `github_token`: Token de autenticación automático que GitHub proporciona a cada workflow
- `publish_dir: ./site`: Especifica que el contenido de la carpeta `site` (generada por MkDocs) debe publicarse en la rama `gh-pages`

---

## Flujo de ejecución completo

1. **Evento disparador**: El desarrollador hace `git push` a la rama `main`
2. **Inicio del workflow**: GitHub detecta el push y activa el workflow
3. **Preparación del entorno**: Se crea una máquina virtual Ubuntu limpia
4. **Clonación**: Se descarga el código del repositorio
5. **Instalación**: Se configura Python y se instalan las dependencias
6. **Generación**: MkDocs procesa los archivos Markdown y crea el sitio web
7. **Publicación**: El contenido se sube a la rama `gh-pages`
8. **Despliegue**: GitHub Pages detecta cambios en `gh-pages` y actualiza la web pública

Todo este proceso toma aproximadamente **30-60 segundos** y ocurre sin intervención manual.

---

## Monitorización y depuración
### Ver ejecuciones del workflow:
1. Ir a la pestaña "Actions" del repositorio
2. Seleccionar el workflow "Generar documentación con MkDocs"
3. Ver el historial de ejecuciones con su estado (success, failed, in progress)

### Revisar logs:
- Cada step del workflow genera logs detallados
- Si algo falla, los logs muestran exactamente dónde y por qué
- Los logs son accesibles desde la interfaz de GitHub Actions

### Estados posibles:
- ✅ **Success**: El workflow se ejecutó correctamente
- ❌ **Failed**: Hubo un error en alguno de los steps
- 🟡 **In progress**: El workflow está ejecutándose actualmente
- ⏸️ **Cancelled**: Se canceló manualmente la ejecución

---

## Ventajas de este enfoque

1. **Documentación siempre actualizada**: Cada cambio en el código actualiza automáticamente la documentación
2. **Sin esfuerzo manual**: No hay que recordar regenerar y publicar la documentación
3. **Consistencia garantizada**: El proceso es siempre idéntico, eliminando errores humanos
4. **Historial completo**: Se puede ver quién y cuándo se actualizó cada versión
5. **Integración con el flujo de desarrollo**: Forma parte natural del proceso de desarrollo
6. **Feedback inmediato**: Si hay errores en la documentación, se detectan al instante

---

## Mejoras y extensiones posibles

### Tests automáticos:
```yaml
- name: Validar enlaces
  run: mkdocs build --strict
```
El flag `--strict` hace que MkDocs falle si hay enlaces rotos.

### Notificaciones:
Se pueden configurar notificaciones por email, Slack o Discord cuando el workflow falla.

### Despliegue condicional:
```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'docs/**'
```
Esto hace que el workflow solo se ejecute si hay cambios en la carpeta `docs/`, ahorrando recursos.

### Múltiples entornos:
Se pueden crear workflows diferentes para desarrollo, staging y producción.

---

## Conclusión

La implementación de GitHub Actions para la generación automática de documentación representa un ejemplo práctico de DevOps y CI/CD. Este enfoque:

- Ahorra tiempo y esfuerzo
- Reduce errores humanos
- Mejora la calidad del proyecto
- Demuestra conocimientos avanzados de automatización
- Es una práctica estándar en la industria del software

Este workflow puede adaptarse y extenderse para automatizar prácticamente cualquier tarea repetitiva en el desarrollo de software.
