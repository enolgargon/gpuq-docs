# Ejecutar contenedores Docker

Una vez definidos el **Dockerfile** y el fichero **docker-compose**, el siguiente paso es ejecutar el experimento.

En esta sección se describen las órdenes básicas de Docker que se utilizarán para ejecutar y gestionar contenedores en el servidor.

No es necesario conocer Docker en profundidad. Solo veremos las órdenes más habituales para trabajar con experimentos.

---

# Ejecutar un experimento

Para ejecutar un experimento definido con `docker-compose` se utiliza el orden:
```bash
docker compose up
```


Esta orden realiza varias acciones:

1. Construye la imagen definida en el `Dockerfile` (si todavía no existe).
2. Crea el contenedor.
3. Ejecuta el experimento dentro del contenedor.

Si el experimento ya se había ejecutado anteriormente y la imagen ya existe, Docker reutilizará esa imagen para lanzar el contenedor.

---

## Seleccionar el fichero docker-compose

Por defecto Docker busca un fichero llamado: `docker-compose.yml`.


Si el proyecto utiliza ese nombre **no es necesario especificar nada más**.

Sin embargo, si el proyecto contiene varios ficheros `docker-compose`, es posible seleccionar uno concreto utilizando la opción `-f`.

Por ejemplo:
```bash
docker compose -f docker-compose.test.yml up
```

Esto permite tener varias configuraciones de ejecución dentro del mismo proyecto, por ejemplo:

- una para ejecutar el experimento completo
- otra para ejecutar pruebas o experimentos rápidos

---

## Ejecutar en segundo plano

Muchos experimentos pueden tardar horas o incluso días.

Para evitar mantener la terminal abierta durante todo ese tiempo se puede utilizar la opción `-d` (*detached mode*):
``` bash
docker compose up -d
```

o, si se quiere especificar el fichero:
```bash
docker compose -f docker-compose.yml up -d
```


En este modo el contenedor se ejecuta **en segundo plano** y es posible cerrar la sesión del terminal y volver más tarde.

---

# Detener un experimento

Para detener los contenedores creados por `docker-compose` se utiliza:
```bash
docker compose down
```

Esta orden:

- detiene los contenedores
- elimina los contenedores creados por `docker-compose`

Es importante tener en cuenta que **la imagen Docker no se elimina**, solo el contenedor.

Esto significa que el experimento puede volver a ejecutarse rápidamente sin tener que reconstruir la imagen.

---

# Ver contenedores

Para ver los contenedores que están ejecutándose se utiliza:
```bash
docker container ls
```

Esta orden muestra información como:

- identificador del contenedor
- imagen utilizada
- estado
- nombre del contenedor
- tiempo de ejecución

---

## Ver todos los contenedores

Por defecto solo se muestran los contenedores que están en ejecución.

Para ver **todos los contenedores**, incluidos los que ya han terminado, se utiliza la opción `-a`:
```bash
docker container ls -a
```


Esto permite comprobar si un experimento ya ha terminado o si ha finalizado con algún error.

---

## Ver el último contenedor

La opción `-l` permite mostrar **el último contenedor creado**:
```bash
docker container ls -l
```


Esto es útil cuando se acaba de ejecutar un experimento y se quiere localizar rápidamente el contenedor correspondiente.

---

# Ver los logs de un contenedor

Para consultar la salida de un contenedor se utiliza:
```bash
docker logs <container_id>
```


Esta orden muestra la salida estándar del programa que se está ejecutando dentro del contenedor.

En experimentos de *machine learning* esto suele incluir:

- progreso del entrenamiento
- métricas
- mensajes de error
- información de depuración

---

## Identificador del contenedor

El identificador del contenedor suele ser una cadena larga, por ejemplo `8f3a1c2b4d9e7c...`.
No es necesario escribir el identificador completo.

Docker permite utilizar **solo los primeros caracteres**, siempre que sean suficientes para diferenciar ese contenedor de los demás.

Por ejemplo:
```bash
docker logs 8f3a
```


Funcionará siempre que no exista otro contenedor cuyo identificador empiece por `8f3a`.

---

# Gestionar imágenes Docker

Las imágenes Docker que se han construido en el sistema pueden listarse con:
```bash
docker image ls
```

Esta orden muestra:

- nombre de la imagen
- identificador
- fecha de creación
- tamaño

Las imágenes contienen el entorno completo necesario para ejecutar el experimento.

---

## Eliminar una imagen

Para eliminar una imagen se utiliza:
```bash
docker image rm <image_id>
```


Eliminar una imagen es **importante y necesario cuando se realizan cambios en el código o en el entorno del experimento**.

Si una imagen ya existe, `docker compose` la reutilizará en ejecuciones posteriores y **no reconstruirá la imagen automáticamente**.

Esto significa que los cambios realizados en el código podrían no reflejarse en el contenedor.

Eliminar la imagen obliga a Docker a reconstruirla la próxima vez que se ejecute el experimento.

---

# Resumen

Las órdenes más utilizadas para trabajar con contenedores son:

| Orden | Descripción |
|---|---|
| `docker compose up -d` | Ejecutar un experimento en segundo plano |
| `docker compose down` | Detener y eliminar los contenedores |
| `docker container ls` | Ver contenedores en ejecución |
| `docker container ls -a` | Ver todos los contenedores |
| `docker container ls -l` | Ver el último contenedor |
| `docker logs <container>` | Ver la salida de un contenedor |
| `docker image ls` | Listar imágenes disponibles |
| `docker image rm <image>` | Eliminar una imagen |
