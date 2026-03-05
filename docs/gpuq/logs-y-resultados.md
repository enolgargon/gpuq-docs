# Logs y resultados

Una vez que un trabajo ha sido enviado a la cola y comienza su ejecución, es habitual querer consultar:

- la salida del experimento
- posibles errores durante la ejecución
- los resultados generados

En esta sección se describe cómo acceder a esta información.

---

# Salida del experimento

Los experimentos enviados mediante `gpuq` se ejecutan dentro de contenedores Docker utilizando la configuración definida en el fichero `docker-compose`.

Por este motivo, la salida del experimento se puede consultar utilizando las herramientas estándar de Docker.

---

## Ver los logs de un contenedor

Para consultar la salida de un contenedor se utiliza la orden:
```bash
docker logs CONTAINER_ID
```

Esta orden muestra la salida estándar del programa que se está ejecutando dentro del contenedor.

En experimentos de *machine learning* esto suele incluir:

- progreso del entrenamiento
- métricas del modelo
- mensajes de depuración
- errores de ejecución

---

## Localizar el contenedor

Para encontrar el contenedor asociado a un experimento se puede utilizar:
```bash
docker container ls -a
```

---

# Resultados del experimento

En la estructura recomendada para los proyectos se utiliza un directorio llamado `results`. Este directorio se monta dentro del contenedor mediante un volumen definido en `docker-compose`.

Esto significa que cualquier fichero que el experimento escriba en `/results` dentro del contenedor aparecerá automáticamente en el directorio `results/` del proyecto.

De esta forma:

- los resultados permanecen en el sistema incluso cuando el contenedor termina
- es posible analizar los resultados sin necesidad de acceder al contenedor

---

# Depuración de errores

Si un experimento falla, los pasos habituales para investigar el problema son:

1. consultar el estado del trabajo con `gpuq list`
2. localizar el contenedor correspondiente
3. revisar los logs del contenedor con `docker logs`

Los mensajes de error del programa suelen aparecer en los logs del contenedor.

---

# Limpieza de contenedores e imágenes

Es importante recordar que `gpuq` **no elimina contenedores ni imágenes Docker**.

Cuando un experimento termina:

- el contenedor creado por Docker permanece en el sistema
- la imagen Docker utilizada para ejecutarlo también permanece

La eliminación de contenedores o imágenes que ya no sean necesarios es responsabilidad del usuario.

Las órdenes necesarias para gestionar estos recursos se describen en la sección sobre uso de Docker.

---

# Resumen

Las herramientas principales para consultar la ejecución de un experimento son:

| Orden | Descripción |
|---|---|
| `gpuq list` | Mostrar el estado de los trabajos en la cola |
| `docker container ls` | Mostrar contenedores en ejecución |
| `docker container ls -a` | Mostrar todos los contenedores |
| `docker logs CONTAINER_ID` | Consultar la salida de un contenedor |

Los resultados del experimento se almacenan en el directorio `results/` del proyecto.