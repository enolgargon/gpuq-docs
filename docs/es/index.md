# Gestión de colas para GPU en experimentos - gpuq

`gpuq` es una herramienta diseñada para gestionar el uso compartido de GPUs en un servidor de investigación.

En muchos servidores utilizados por estudiantes y grupos de investigación, varias personas ejecutan experimentos de *machine learning* al mismo tiempo. Esto suele provocar problemas como:

- varios usuarios intentando usar la misma GPU
- experimentos que fallan porque la GPU está ocupada
- dificultad para reproducir experimentos en distintos entornos

`gpuq` resuelve estos problemas utilizando:

- **Docker** para definir el entorno de ejecución de los experimentos
- **docker-compose** para describir cómo se ejecuta cada experimento
- **una cola de trabajos** que garantiza que solo un experimento utilice la GPU a la vez

---

## Qué aprenderás en esta documentación

Esta documentación explica paso a paso cómo:

1. Entender qué es Docker y por qué se utiliza en experimentación
2. Definir un experimento usando `Dockerfile` y `docker-compose`
3. Ejecutar experimentos en el servidor utilizando `gpuq`

No es necesario tener experiencia previa con Docker.

---

## Flujo de trabajo

El flujo típico de trabajo es el siguiente:

1. Definir el entorno del experimento con un **Dockerfile**
2. Definir cómo se ejecuta el experimento con **docker-compose**
3. Enviar el experimento a la cola utilizando **gpuq**


---

## Estructura de un experimento

En esta documentación utilizaremos una estructura sencilla para los experimentos:
```plain
experimento/
├── Dockerfile
├── docker-compose.yml
├── train.py
├── requirements.txt
├── data/
└── results/
```

Donde:

- **Dockerfile** define el entorno de ejecución
- **docker-compose.yml** define cómo se ejecuta el experimento
- **data/** contiene los datos de entrada
- **results/** contiene los resultados generados
