# Conceptos básicos de Docker

En esta sección se introducen algunos conceptos básicos de Docker que son necesarios para entender cómo se ejecutan los experimentos en este servidor.

No es necesario conocer Docker en profundidad. Solo veremos los conceptos mínimos que se utilizarán en los ejemplos de esta documentación.

---

## Imagen

Una **imagen Docker** es una plantilla que contiene todo lo necesario para ejecutar un programa.

Una imagen puede incluir:

- un sistema operativo base
- librerías del sistema
- dependencias de Python
- herramientas necesarias para el experimento


Las imágenes se utilizan como base para crear contenedores.

---

## Contenedor

Un **contenedor** es una instancia en ejecución de una imagen.

Podemos pensar en la relación entre imágenes y contenedores de forma similar a:

- imagen → plantilla
- contenedor → programa en ejecución


Cuando ejecutamos un experimento con Docker, en realidad lo que ocurre es que Docker crea un contenedor a partir de una imagen y ejecuta el programa dentro de él.

Cada contenedor está **aislado del sistema y de otros contenedores**.

Esto permite que distintos experimentos utilicen dependencias distintas sin interferir entre sí.

---

## Dockerfile

Un **Dockerfile** es un fichero que describe cómo construir una imagen Docker.

En él se definen aspectos como:

- la imagen base
- las dependencias que se deben instalar
- el directorio de trabajo
- la orden que ejecuta el experimento

Un ejemplo simple de Dockerfile podría ser:

```dockerfile
FROM python:3.10

WORKDIR /workspace

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY train.py .

CMD ["python", "train.py"]
```

Este fichero indica a Docker cómo construir la imagen que se utilizará para ejecutar el experimento.

En la siguiente sección veremos con más detalle cómo escribir un Dockerfile.

---

## Volúmenes

Los volúmenes permiten compartir ficheros entre el sistema anfitrión (el servidor) y el contenedor.

Esto es especialmente útil para trabajar con:

- datasets
- resultados de experimentos
- ficheros de configuración

Por ejemplo, podemos montar un directorio del servidor dentro del contenedor: `./data → /data`

De esta forma el contenedor puede leer los datos desde /data y los resultados que escriba el contenedor permanecen en el sistema del servidor.

Esto permite separar claramente:

- el entorno del experimento (dentro del contenedor)
- los datos y resultados (fuera del contenedor)

---

## Variables de entorno

Las variables de entorno permiten pasar parámetros al experimento cuando se ejecuta el contenedor.

Por ejemplo, podemos definir parámetros como:

```plain
EPOCHS=50
LEARNING_RATE=0.001
```

El programa puede leer estas variables para configurar su comportamiento.

Esto permite cambiar parámetros del experimento sin modificar el código.