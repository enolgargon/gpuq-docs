# Ejemplo mínimo

En esta sección se muestra un ejemplo mínimo de cómo ejecutar un experimento utilizando los ficheros descritos en las secciones anteriores.

Un proyecto típico puede tener una estructura como la siguiente:
```plain
experiment/
├── Dockerfile
├── docker-compose.yml
├── docker-compose.test.yml
├── requirements.txt
├── main.py
├── model.py
├── test.py
├── data/
└── results/
```


El experimento puede ejecutarse de dos formas distintas dependiendo de si necesita GPU o no.

- Experimentos **sin GPU** → ejecutar directamente con `docker compose`
- Experimentos **con GPU** → enviar a la cola con `gpuq`

---

# Ejecutar un experimento sin GPU

Si el experimento no necesita GPU, puede ejecutarse directamente con Docker.

Por ejemplo, para ejecutar el experimento definido en `docker-compose.yml`:
```bash
docker compose up -d
```

Esta orden:

1. construye la imagen definida en el `Dockerfile` (si no existe)
2. crea el contenedor
3. ejecuta el experimento en segundo plano

---

# Ejecutar un experimento con GPU

Los experimentos que utilizan GPU **no deben ejecutarse directamente con Docker**.

En su lugar deben enviarse a la cola utilizando **gpuq**.

Esto garantiza que:

- solo un experimento utilice la GPU a la vez
- los experimentos se ejecuten en orden
- se mantenga un registro persistente de los trabajos ejecutados

---

## Enviar un trabajo a la cola

Para enviar un experimento a la cola se utiliza la orden:
```bash
gpuq submit --project PATH
```

donde `PATH` es el directorio que contiene el proyecto.

Por ejemplo:
```bash
gpuq submit --project ./experiment-name
```

Esta orden:

- registra el trabajo en la cola
- genera un identificador único para el trabajo
- asocia el trabajo con el usuario que lo ha enviado

El trabajo queda en estado **queued** hasta que el sistema lo ejecute.

---

## Seleccionar el fichero docker-compose

Por defecto `gpuq` utilizará el fichero `docker-compose.yml`. 
Si se desea utilizar otro fichero `docker-compose`, se puede indicar con la opción `--compose`.

Por ejemplo:
```bash
gpuq submit --project ./experiment --compose docker-compose.test.yml
```

# Listar trabajos en la cola

Para ver los trabajos registrados en la cola se utiliza:
```bash
gpuq list
```

Esta orden muestra información como:

- identificador del trabajo
- usuario que lo envió
- fecha de creación
- descripción

---

# Cancelar un trabajo

Para cancelar un trabajo se utiliza:
```bash
gpuq cancel JOB_ID
```

Por ejemplo:
```bash
gpuq cancel job-1a2b3c4d
```

Si el trabajo está en estado **queued**, se moverá al estado **canceled**.

Si el trabajo está en estado **running**, el sistema marcará el trabajo como cancelado y el proceso de ejecución se encargará de detenerlo.
