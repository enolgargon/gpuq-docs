# Usar docker-compose

Mientras que el **Dockerfile** define el entorno del experimento, el fichero **docker-compose.yml** define **cómo se ejecuta el experimento**.

En él se especifican aspectos como:

- qué imagen se utilizará
- qué variables de entorno se definen
- qué directorios se comparten con el contenedor
- si el experimento utiliza GPU

Este fichero describe completamente cómo debe ejecutarse el experimento.

---

## Ejemplo de docker-compose

Un fichero `docker-compose.yml` típico para un experimento puede tener el siguiente aspecto:

```yaml
services:

  experiment:

    build: .

    runtime: nvidia
    deploy: 
      resources: 
        reservations: 
          devices: 
            - driver: nvidia 
              device_ids: ['0'] 
              capabilities: [gpu]

    environment:
      - EPOCHS=50
      - LEARNING_RATE=0.001

    volumes:
      - ./data:/data:ro
      - ./results:/results
```

Este fichero define el contenedor que ejecutará el experimento.

---

## Construcción de la imagen

La opción build indica que Docker debe construir la imagen utilizando el Dockerfile del directorio actual.

```yaml
build: .
```

Esto significa que antes de ejecutar el experimento, Docker construirá la imagen definida por el Dockerfile.

---

## Uso de GPU

Para que el contenedor pueda utilizar la GPU del servidor se utiliza:

```yaml
runtime: nvidia
deploy: 
  resources: 
    reservations: 
      devices: 
        - driver: nvidia 
          device_ids: ['0'] 
          capabilities: [gpu] 
```

Esto permite que el contenedor acceda a CUDA y a la GPU disponible.

---

## Variables de entorno

Las variables de entorno permiten definir parámetros del experimento.

```yaml
environment:
  - EPOCHS=50
  - LEARNING_RATE=0.001
```

Estas variables estarán disponibles dentro del contenedor y el programa puede leerlas para configurar su comportamiento.

Esto permite cambiar la configuración del experimento sin modificar el código ni reconstruir la imagen Docker.

---

## Volúmenes

Los volúmenes permiten compartir directorios entre el servidor y el contenedor.

```yaml
volumes:
  - ./data:/data:ro
  - ./results:/results
```

En este ejemplo:

- `./data` contiene los datos de entrada del experimento
- `./results` se utiliza para guardar los resultados

Observa que la definición del volumen de datos contiene al final la etiqueta `:ro`. Eso quiere decir que ese volumen se montará en modo sólo lectura. El contenedor podrá ver esos datos, los podrá abrir y podrá operar con ellos, pero no podrá escribir en ese directorio. Eso protege los datos de ser modificados de forma accidentarl.

En el caso del directorio de resultados, no se ha incluido esta configuración. Por defecto, si no se indica, el directorio permitirá tanto lectura como escritura.

Dentro del contenedor estos directorios estarán disponibles como:

```plain
/data
/results
```

Esto permite que los resultados generados por el contenedor permanezcan en el sistema del servidor.