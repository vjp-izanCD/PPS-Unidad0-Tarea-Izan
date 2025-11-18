# Publicación con GitHub Pages

En este documento se detalla el proceso completo para activar y configurar GitHub Pages, permitiendo publicar la documentación del proyecto de forma accesible y profesional en internet.

---

## ¿Qué es GitHub Pages?

GitHub Pages es un servicio de alojamiento web estático gratuito ofrecido por GitHub que permite publicar sitios web directamente desde un repositorio. Es especialmente útil para:

- **Documentación de proyectos**: Como en nuestro caso
- **Portfolios personales**: Mostrar proyectos y habilidades
- **Blogs técnicos**: Usando generadores como Jekyll o MkDocs
- **Páginas de proyectos open source**: Presentación pública de herramientas

### Características principales:
- ✅ **Completamente gratuito** para repositorios públicos
- ✅ **Dominio propio disponible**: Puedes usar tu dominio personalizado
- ✅ **HTTPS automático**: Seguridad incluida sin configuración adicional
- ✅ **Actualización instantánea**: Cambios reflejados en segundos/minutos
- ✅ **Integración total con Git**: Control de versiones de tu web

---

## Proceso de configuración paso a paso

### 1. Acceder a la configuración del repositorio

**Pasos**:
1. Navegar al repositorio en GitHub
2. Hacer clic en la pestaña **"Settings"** (Configuración) en la parte superior
3. En el menú lateral izquierdo, buscar y hacer clic en **"Pages"**

**Justificación**: La sección de Pages contiene todas las opciones necesarias para configurar la publicación del sitio web, incluyendo la selección de rama, carpeta y dominio personalizado.

---

### 2. Configurar la fuente de publicación
**Configuración**:
- **Branch (Rama)**: `gh-pages` 
- **Folder (Carpeta)**: `/ (root)`

**Justificación**:
- La rama `gh-pages` es creada automáticamente por nuestro workflow de GitHub Actions
- Esta rama contiene únicamente los archivos HTML/CSS/JS generados por MkDocs
- Separar el código fuente (rama `main`) del contenido publicado (rama `gh-pages`) es una buena práctica que mantiene el repositorio organizado
- La carpeta root (`/`) indica que todos los archivos de la rama serán publicados

---

### 3. Guardar configuración

**Acción**: Hacer clic en el botón **"Save"**

**Justificación**: Al guardar, GitHub Pages comienza el proceso de despliegue. Este proceso:
1. Valida la configuración
2. Lee los archivos de la rama especificada
3. Genera la infraestructura necesaria
4. Publica el sitio en los servidores de GitHub

Este proceso inicial puede tardar entre 1-5 minutos.

**Evidencias de GitHub Pages configurado:**

**Página de configuración:**

![GitHub Pages Settings](https://raw.githubusercontent.com/vjp-izanCD/PPS-Unidad0-Tarea-Izan/main/images/Captura7.png)

---

### 4. Verificar el despliegue

**Indicadores de éxito**:
- Aparece un mensaje verde indicando: *"Your site is published at https://vjp-izancd.github.io/PPS-Unidad0-Tarea-Izan/"*
- Se puede ver el enlace clickeable a la web
- El estado muestra un check verde ✅

**Justificación**: Esta confirmación visual garantiza que:
- La configuración es correcta
- El sitio está accesible públicamente
- El dominio generado sigue el patrón: `https://[usuario].github.io/[repositorio]/`

---

## Estructura de URLs de GitHub Pages

### Para cuentas de usuario:
```
https://[usuario].github.io/[repositorio]/
```

**Ejemplo de este proyecto**:
```
https://vjp-izancd.github.io/PPS-Unidad0-Tarea-Izan/
```

### Desglose:
- `vjp-izancd`: Nombre de usuario de GitHub
- `github.io`: Dominio base de GitHub Pages
- `PPS-Unidad0-Tarea-Izan`: Nombre del repositorio

---

## Integración con el Workflow

Nuestro workflow de GitHub Actions y GitHub Pages trabajan juntos de la siguiente manera:

### Flujo completo:

1. **Desarrollador hace push** a la rama `main` con cambios en la documentación
2. **GitHub Actions se activa** automáticamente
3. **El workflow ejecuta**:
   - Instala dependencias (MkDocs)
   - Genera archivos HTML estáticos
   - Hace push a la rama `gh-pages`
4. **GitHub Pages detecta** cambios en `gh-pages`
5. **GitHub Pages redespliega** el sitio automáticamente
6. **La web se actualiza** en cuestión de segundos

### Diagrama del flujo:
```
[Push a main] → [GitHub Actions] → [mkdocs build] → [Push a gh-pages] → [GitHub Pages] → [Web Pública]
```

---

## Ventajas de usar GitHub Pages

### 1. **Hosting gratuito e ilimitado**
- Sin costes de servidores
- Sin límites de tráfico para uso normal
- Ancho de banda generoso (100GB/mes recomendado)

### 2. **Mantenimiento cero**
- No hay servidores que configurar
- No hay actualizaciones de seguridad que aplicar
- GitHub se encarga de la infraestructura

### 3. **Rendimiento excelente**
- CDN global de GitHub
- Carga rápida desde cualquier parte del mundo
- Optimización automática de recursos

### 4. **HTTPS incluido**
- Certificado SSL gratuito y automático
- Renovación automática
- Seguridad sin esfuerzo adicional

### 5. **Control de versiones integrado**
- Cada cambio queda registrado en Git
- Fácil revertir a versiones anteriores
- Historial completo de modificaciones

### 6. **Integración perfecta con el ecosistema GitHub**
- Funciona nativamente con GitHub Actions
- Vinculado directamente al repositorio
- Sin configuraciones complejas

---

## Monitorización y gestión
### Ver el estado del despliegue:

1. **En la página de Settings > Pages**:
   - Muestra el estado actual (publicado, en proceso, error)
   - Enlace directo al sitio web
   - Última fecha de actualización

2. **En la pestaña Actions**:
   - Ver ejecuciones del workflow de publicación
   - Logs detallados de cada despliegue
   - Historial completo de actualizaciones

3. **En la pestaña Environments** (si está habilitada):
   - Historial de despliegues a `github-pages`
   - Estado de cada versión publicada
   - URL de cada despliegue

---

## Solución de problemas comunes

### Problema 1: "404 - No se encuentra la página"
**Causas posibles**:
- La rama `gh-pages` está vacía
- No existe un archivo `index.html`
- La configuración de Pages no está guardada

**Solución**:
- Verificar que el workflow de Actions se ejecutó correctamente
- Comprobar que la rama `gh-pages` tiene contenido
- Revisar la configuración en Settings > Pages

---

### Problema 2: "Los cambios no se reflejan"
**Causas posibles**:
- Caché del navegador
- El despliegue aún no ha terminado
- Error en el workflow de Actions

**Solución**:
- Forzar actualización con `Ctrl + F5` o `Cmd + Shift + R`
- Esperar 1-2 minutos para que se complete el despliegue
- Revisar la pestaña Actions en busca de errores

---

### Problema 3: "Estilos CSS no se cargan correctamente"
**Causa**:
- Rutas absolutas incorrectas en el código HTML

**Solución**:
- Asegurar que MkDocs está configurado con `site_url` correcto en `mkdocs.yml`
- Usar rutas relativas en los archivos Markdown

---

## Personalización avanzada

### Dominio personalizado

Si tienes un dominio propio, puedes configurarlo:

1. En Settings > Pages, agregar el dominio en "Custom domain"
2. En tu proveedor DNS, crear un registro CNAME apuntando a `[usuario].github.io`
3. Esperar a que se verifique el dominio (puede tardar hasta 24 horas)

**Ventajas**:
- URL profesional y fácil de recordar
- Mayor credibilidad del proyecto
- Mejor posicionamiento SEO

---

### Forzar HTTPS

**Recomendación**: Siempre activar la opción "Enforce HTTPS" en Settings > Pages

**Justificación**:
- Mayor seguridad para los visitantes
- Mejor ranking en motores de búsqueda
- Cumplimiento de estándares web modernos
- Protección contra ataques man-in-the-middle

---

## Enlace de la documentación publicada

### URL del proyecto:
```
https://vjp-izancd.github.io/PPS-Unidad0-Tarea-Izan/
```

**Este enlace debe incluirse en**:
- El archivo README.md del repositorio
- La descripción del repositorio en GitHub
- La entrega de la tarea en la plataforma educativa

---

## Conclusión

GitHub Pages es una herramienta imprescindible para cualquier desarrollador que quiera:

- Compartir documentación de forma profesional
- Publicar portfolios y proyectos personales
- Demostrar habilidades técnicas
- Colaborar en proyectos open source

La combinación de **MkDocs + GitHub Actions + GitHub Pages** crea un pipeline completamente automatizado que:

✅ Ahorra tiempo
✅ Reduce errores
✅ Garantiza consistencia
✅ Es gratuito
✅ Requiere configuración mínima
✅ Es mantenible a largo plazo

Esta arquitectura representa las mejores prácticas modernas de desarrollo y documentación de software.
