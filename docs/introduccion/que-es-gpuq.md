# ¿Qué es gpuq?

`gpuq` es una herramienta diseñada para gestionar el uso compartido de GPUs en un servidor de investigación.

En muchos grupos de investigación, varias personas utilizan el mismo servidor para ejecutar experimentos de *machine learning*. Estos experimentos suelen requerir el uso de GPU para poder entrenar modelos en un tiempo razonable.

Cuando varios usuarios comparten el mismo servidor, es habitual encontrarse con problemas como:

- varios usuarios intentando utilizar la misma GPU al mismo tiempo
- experimentos que fallan porque la GPU ya está ocupada
- dificultad para coordinar quién puede usar la GPU en cada momento

`gpuq` resuelve este problema mediante **una cola de trabajos** que gestiona la ejecución de experimentos.

---

## Cómo funciona gpuq

El funcionamiento de `gpuq` es sencillo.

Los usuarios **envían sus experimentos a una cola**, en lugar de ejecutarlos directamente en el servidor.

`gpuq` se encarga de ejecutar los experimentos **uno a uno**, garantizando que solo un experimento esté utilizando la GPU en cada momento.

El flujo de trabajo es el siguiente:

1. El usuario define su experimento utilizando Docker
2. El usuario envía el experimento a la cola usando `gpuq`
3. `gpuq` ejecuta los experimentos en orden de llegada

De esta forma se evita que varios experimentos compitan por la misma GPU.

---

## Relación entre Docker y gpuq

En este servidor, los experimentos se ejecutan utilizando **contenedores Docker**.

Esto significa que cada experimento define su entorno de ejecución mediante:

- un **Dockerfile**, que define el entorno del experimento
- un **docker-compose.yml**, que describe cómo se ejecuta

`gpuq` utiliza estos ficheros para lanzar el experimento dentro de un contenedor.

Esto tiene varias ventajas:

- cada experimento se ejecuta en un entorno aislado
- no hay conflictos entre dependencias
- los experimentos son reproducibles

---

## Flujo de trabajo completo

El flujo típico de trabajo para ejecutar un experimento en el servidor es el siguiente:

1. Definir el entorno del experimento con un **Dockerfile**
2. Definir la ejecución del experimento con **docker-compose**
3. Enviar el experimento a la cola con **gpuq**


En las siguientes secciones aprenderás cómo:

- escribir un **Dockerfile**
- configurar un **docker-compose.yml**
- ejecutar experimentos utilizando **gpuq**