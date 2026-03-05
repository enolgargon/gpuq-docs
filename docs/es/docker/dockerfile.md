# Crear un Dockerfile

Un **Dockerfile** es un fichero que describe cómo construir la imagen Docker que se utilizará para ejecutar un experimento.

En este fichero se define:

- la imagen base
- las dependencias necesarias
- el código que se ejecutará
- la orden que lanza el experimento

A partir del Dockerfile, Docker construye una **imagen** que luego se utilizará para crear el contenedor donde se ejecutará el experimento.

---

## Estructura básica de un Dockerfile

Un Dockerfile típico para un experimento puede tener una estructura como esta:

```dockerfile
FROM pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime

WORKDIR /workspace

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY train.py .

CMD ["python", "train.py"]
```

Este fichero define el entorno necesario para ejecutar el experimento.

---

## Imagen base

La instrucción FROM indica qué imagen se utilizará como base.

```dockerfile
FROM pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime
```

Las imágenes base suelen incluir:

- Python
- bibliotecas de machine learning

Elegir una buena imagen base permite evitar tener que instalar muchas dependencias manualmente.

---

## Directorio de trabajo

La instrucción WORKDIR define el directorio desde el que se ejecutarán las órdenes dentro del contenedor.

```dockerfile
WORKDIR /workspace
```

A partir de este punto, los ficheros se copiarán y las órdenes se ejecutarán en ese directorio.

## Instalación de dependencias

Normalmente las dependencias de Python se definen en un fichero requirements.txt.

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt
```

Primero se copia el fichero al contenedor y después se instalan las dependencias.

Esto permite que el entorno del experimento quede completamente definido.

--

## Copiar el código

Después se copia el código del experimento.

```dockerfile
COPY train.py .
```

Esto hace que el fichero esté disponible dentro del contenedor.

## Orden de ejecución

La instrucción CMD define qué orden se ejecutará cuando se inicie el contenedor.

```dockerfile
CMD ["python", "train.py"]
```

En este caso, el contenedor ejecutará el script train.py.

---

## Buenas prácticas
### Evitar COPY . .

Es habitual encontrar Dockerfiles que contienen:

```dockerfile
COPY . .
```

Esto copia todo el directorio del proyecto dentro del contenedor.

En experimentación esto suele ser una mala práctica porque puede incluir:

- datasets
- resultados de experimentos
- ficheros innecesarios

Es preferible copiar solo los ficheros necesarios para ejecutar el experimento.

### Separar código y datos

Los datasets y resultados no deberían copiarse dentro de la imagen Docker.

En su lugar, se utilizarán volúmenes para montar esos directorios cuando se ejecute el contenedor.

Esto permite:

- reutilizar la misma imagen para distintos datasets
- evitar imágenes Docker muy grandes
- mantener separados código y datos

Veremos cómo hacerlo en la sección de `docker-compose`

### Usar variables de entorno para la configuración

En muchos experimentos es necesario cambiar parámetros como:

- número de épocas
- tasa de aprendizaje
- tamaño de batch
- rutas de entrada o salida

Una mala práctica común es modificar el código cada vez que se quiere cambiar un parámetro y volver a construir la imagen Docker.

Esto implica volver a ejecutar el proceso de construcción de la imagen, lo que puede ser lento e innecesario.

En su lugar, es recomendable utilizar variables de entorno para definir estos parámetros.

Por ejemplo, el código del experimento puede leer valores como:

- EPOCHS
- LEARNING_RATE
- BATCH_SIZE

Estas variables pueden definirse cuando se ejecuta el contenedor.

Por ejemplo:

```plain
EPOCHS=50
LEARNING_RATE=0.001
```

De esta forma:

- el código del experimento no cambia
- no es necesario reconstruir la imagen
- se pueden lanzar múltiples experimentos con configuraciones distintas

Esto permite reutilizar la misma imagen Docker para diferentes configuraciones del experimento.