---
description: Guía paso a paso para levantar el contenedor Docker del Worker Linear en tu máquina local.
---

# MANUAL DE DESPLIEGUE LOCAL DOCKER (WORKER LINEAR)

## 1. Objetivo de la Guía
Este manual te enseñará a compilar y ejecutar de forma local el Worker (microservicio) de Linear utilizando Docker. Al finalizar, tendrás el servidor ejecutándose en tu ordenador y podrás verificar que responde correctamente a las peticiones, lo cual es útil para realizar pruebas antes de subirlo a la nube (Railway).

## 2. Requisitos Previos
- [ ] Tener **Docker Desktop** instalado y en ejecución en tu ordenador (deberías ver el icono de la ballena en tu barra de tareas).
- [ ] Tener la terminal (o consola) abierta.

## 3. Variables de Entorno (Obligatorio)
Para ejecutar la imagen base en local necesitas inyectar un archivo `.env`. El contenedor utilizará el puerto 8000 por defecto y **fallará de forma segura (Crash / Error 500)** si no detecta las variables de entorno requeridas (ej: `LINEAR_API_KEY`, `LINEAR_TEAM_ID`). Asegúrate de tener tu archivo `.env` configurado dentro de la carpeta `apps/worker_linear_agent`.

## 4. Instrucciones Paso a Paso (Modo Principiante)

### Paso 1: Navegar a la carpeta del Worker
1. Abre tu terminal.
2. Debes asegurarte de estar dentro de la carpeta donde se encuentra el código del Worker. Ejecuta este comando exacto copiándolo y pegándolo:
   ```bash
   cd apps/worker_linear_agent
   ```
3. **Comprobación:** Sabrás que ha funcionado si la ruta que muestra tu terminal termina en `.../apps/worker_linear_agent`.

### Paso 2: Crear (Compilar) la Imagen de Docker
1. Una vez en la carpeta correcta, vamos a "crear" la imagen de Docker a partir de las instrucciones del código. Ejecuta el siguiente comando:
   ```bash
   docker build -t worker-linear .
   ```
   *(Importante: no olvides copiar el punto `.` al final del comando, significa "en esta carpeta").*
2. **Comprobación:** Sabrás que ha funcionado si en la consola ves una serie de mensajes de descarga y al final termina con un mensaje de éxito como:
   > `Successfully tagged worker-linear:latest` o `DONE`

### Paso 3: Levantar el Contenedor (Encender el servidor)
1. Ahora que tenemos la imagen creada, vamos a encenderla en modo "silencioso" (en segundo plano) para que no bloquee tu terminal. Además, vamos a indicarle explícitamente a Docker que lea tus variables de entorno con `--env-file`. Ejecuta este comando copiándolo y pegándolo:
   ```bash
   docker run -d -p 8000:8000 --env-file .env worker-linear
   ```
2. **Comprobación:** Sabrás que ha funcionado si en la consola ves un texto muy largo (un código de letras y números), que es el Identificador de tu contenedor recién encendido. Por ejemplo:
   > `a1b2c3d4e5f6...`

### Paso 4: Comprobar que el servidor está funcionando
1. Vamos a enviar un mensaje a tu nuevo servidor para ver si responde. Ejecuta este comando en la misma terminal:
   ```bash
   curl http://localhost:8000/docs
   ```
   *(Alternativa: abre en tu navegador de internet la dirección `http://localhost:8000/docs`)*
2. **Comprobación:** Sabrás que ha funcionado si al abrirlo en el navegador ves una página que dice "FastAPI" con documentación interactiva o si en tu consola ves mucho código.

## 5. Solución de Problemas Frecuentes (FAQ)
- **Problema:** La terminal dice `Cannot connect to the Docker daemon` o `docker: command not found`.
  - *Solución:* Significa que Docker Desktop no está encendido o instalado. Abre la aplicación "Docker Desktop" en tu ordenador, espera a que cargue completamente, y vuelve a intentar compilar la imagen.
- **Problema:** En el Paso 3 dice `port is already allocated` o `address already in use`.
  - *Solución:* Significa que ya hay otro programa o contenedor de Docker usando el puerto 8000 en tu ordenador. Ejecuta `docker stop $(docker ps -q)` para detener todos los contenedores y luego vuelve a intentar el Paso 3.
