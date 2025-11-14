# Despliegue local con Docker y NGINX

En esta sección se documenta el proceso completo para desplegar la documentación generada por MkDocs utilizando un contenedor Docker con NGINX como servidor web.

---

## ¿Qué es Docker?

Docker es una plataforma de virtualización a nivel de contenedor que permite empaquetar aplicaciones con todas sus dependencias en unidades estandarizadas llamadas contenedores.

### Ventajas de usar Docker:
- ✅ **Portabilidad**: "Funciona en mi máquina" se convierte en "funciona en todas las máquinas"
- ✅ **Aislamiento**: Cada contenedor está aislado del sistema anfitrión
- ✅ **Ligereza**: Más rápido y eficiente que máquinas virtuales tradicionales
- ✅ **Reproducibilidad**: Entornos consistentes entre desarrollo y producción
- ✅ **Escalabilidad**: Fácil replicar y escalar servicios

---

## ¿Qué es NGINX?

NGINX es un servidor web de alto rendimiento, también usado como proxy inverso, balanceador de carga y caché HTTP.

### Por qué usar NGINX:
- Excelente rendimiento para contenido estático
- Bajo consumo de recursos
- Configuración simple y potente
- Muy usado en producción

---

## Comando de despliegue

### Comando completo:
```bash
docker run --name PPSUnidad0-TareaIzan -d -p 8085:80 -v "$(pwd)/site:/usr/share/nginx/html" nginx
```

---

## Desglose y explicación del comando

### 1. `docker run`
**Justificación**: Comando base para crear y ejecutar un nuevo contenedor.

### 2. `--name PPSUnidad0-TareaIzan`
**Justificación**: Asigna un nombre identificativo al contenedor. Facilita su gestión posterior (parar, reiniciar, eliminar).

### 3. `-d` (detached mode)
**Justificación**: Ejecuta el contenedor en segundo plano, liberando la terminal.

### 4. `-p 8085:80`
**Justificación**: 
- Mapea el puerto 8085 de tu máquina al puerto 80 del contenedor
- Permite acceder a la web desde `http://localhost:8085`
- El puerto 80 es el puerto por defecto de NGINX dentro del contenedor

### 5. `-v "$(pwd)/site:/usr/share/nginx/html"`
**Justificación**:
- Crea un volumen (bind mount) entre tu máquina y el contenedor
- `$(pwd)/site`: Carpeta local donde MkDocs genera la documentación
- `/usr/share/nginx/html`: Carpeta donde NGINX busca archivos para servir
- Cualquier cambio en `site/` se refleja instantáneamente en el contenedor

### 6. `nginx`
**Justificación**: Especifica la imagen oficial de NGINX desde Docker Hub.

---

## Verificar el despliegue

### 1. Comprobar que el contenedor está corriendo:
```bash
docker ps
```

**Salida esperada**: Debe aparecer el contenedor `PPSUnidad0-TareaIzan` en estado `Up`.

### 2. Acceder a la documentación:
Abrir navegador y visitar:
```
http://localhost:8085
```

### 3. Inspeccionar el contenedor:
```bash
docker inspect PPSUnidad0-TareaIzan
```

**Justificación**: Muestra toda la configuración del contenedor (puertos, volúmenes, red, etc.).

---

## Comandos útiles de gestión

### Detener el contenedor:
```bash
docker stop PPSUnidad0-TareaIzan
```

### Reiniciar el contenedor:
```bash
docker restart PPSUnidad0-TareaIzan
```

### Ver logs del contenedor:
```bash
docker logs PPSUnidad0-TareaIzan
```

### Eliminar el contenedor:
```bash
docker rm -f PPSUnidad0-TareaIzan
```

### Acceder al interior del contenedor:
```bash
docker exec -it PPSUnidad0-TareaIzan /bin/bash
```

---

## ¿Por qué desplegar localmente si ya está en GitHub Pages?

Aunque la documentación ya está publicada en GitHub Pages, desplegarla localmente con Docker tiene varios propósitos:

### 1. **Aprendizaje de Docker**
Demuestra conocimiento de:
- Contenedores y virtualización
- Servidores web (NGINX)
- Mapeo de puertos
- Gestión de volúmenes

### 2. **Desarrollo local**
- Previsualizar cambios antes de hacer push
- Probar la documentación sin conexión a internet
- Desarrollo más rápido sin esperar a GitHub Actions

### 3. **Experiencia práctica**
- Simula entornos de producción reales
- Práctica con herramientas DevOps
- Base para proyectos más complejos

### 4. **Requisito de la tarea**
- Cumple con los criterios de evaluación
- Demuestra competencias técnicas

---

## Capturas requeridas

Para la entrega de la tarea, se deben incluir capturas de pantalla que muestren:

1. **Ejecución del comando `docker run`**: Terminal mostrando el comando completo y el usuario
2. **Navegador con la documentación en localhost:8085**: URL visible y contenido cargado
3. **Salida de `docker inspect`**: Mostrando la configuración completa del contenedor
4. **Comando `docker ps`**: Listado de contenedores activos

**Importante**: Todas las capturas deben ser a pantalla completa y mostrar claramente tu nombre de usuario.

---

## Conclusión

El despliegue de la documentación con Docker y NGINX:

✅ Demuestra conocimientos de contenedorización
✅ Proporciona un entorno de desarrollo local
✅ Simula despliegues en producción
✅ Complementa el despliegue en GitHub Pages
✅ Es una habilidad fundamental en DevOps moderno

Esta configuración puede escalarse fácilmente a entornos más complejos con docker-compose, orquestación con Kubernetes, o despliegues en la nube.
