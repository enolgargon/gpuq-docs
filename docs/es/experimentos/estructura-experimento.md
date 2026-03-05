# Estructura de un experimento

En esta sección se describe una estructura sencilla y recomendada para organizar un experimento que se vaya a ejecutar en el servidor.

La idea es separar claramente tres elementos distintos:

- **el entorno de ejecución**
- **el código del experimento**
- **los datos y resultados**

Esta separación facilita:

- reproducir experimentos
- cambiar configuraciones fácilmente
- evitar reconstrucciones innecesarias de imágenes Docker

---

## Estructura recomendada

Un experimento típico puede tener una estructura como la siguiente:
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


Donde:

| Fichero o directorio | Descripción |
|---|---|
| `Dockerfile` | Define el entorno de ejecución del experimento |
| `docker-compose.yml` | Define cómo se ejecuta el experimento principal |
| `docker-compose.test.yml` | Variante para ejecutar pruebas |
| `requirements.txt` | Dependencias de Python |
| `main.py` | Script principal del experimento |
| `model.py` | Definición del modelo |
| `test.py` | Script para pruebas o experimentos rápidos |
| `data/` | Datos de entrada |
| `results/` | Resultados generados por el experimento |

---

## Dockerfile

El **Dockerfile** define el entorno en el que se ejecutará el experimento.

Esto incluye:

- la imagen base
- las dependencias
- los ficheros necesarios
- la orden por defecto

:material-download: [Descargar Dockerfile](../../../snippets/Dockerfile)

```dockerfile
--8<-- "snippets/Dockerfile"
```

Este fichero se utiliza para construir la imagen Docker que ejecutará el experimento.

---

## docker-compose

El fichero docker-compose define cómo se ejecuta el experimento.

En él se especifican aspectos como:

- cómo construir la imagen
- qué variables de entorno utilizar
- qué directorios montar como volúmenes
- si el experimento utiliza GPU

---

## Ejecución del experimento principal

Este fichero ejecuta el experimento principal utilizando main.py.

:material-download: [Descargar docker-compose.yml](../../../snippets/docker-compose.yml)
```yaml
--8<-- "snippets/docker-compose.yml"
```

---

## Ejecución de pruebas

A veces es útil ejecutar una variante del experimento, por ejemplo para:

- probar cambios rápidamente
- ejecutar tests
- validar que el entorno funciona correctamente

Para ello podemos crear un segundo fichero docker-compose.


:material-download: [Descargar docker-compose.test.yml](../../../snippets/docker-compose.test.yml)
```yaml
--8<-- "snippets/docker-compose.test.yml"
```

Este fichero puede modificar aspectos como:

- el script que se ejecuta. *IMPORTANTE:* Observa como se utiliza el campo `command` para cambiar el script que se ejecuta al lanzar el experimento 
- los parámetros del experimento
- configuraciones específicas para pruebas