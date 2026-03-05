# Ejecutar trabajos con gpuq

`gpuq` es una herramienta diseñada para enviar experimentos que utilizan GPU a una cola de ejecución en un servidor compartido.

En lugar de ejecutar directamente un contenedor con Docker, los experimentos que requieren GPU deben enviarse a la cola mediante `gpuq`. Esto permite controlar el uso de la GPU y evitar que varios experimentos intenten utilizarla al mismo tiempo.

Cada trabajo enviado a la cola queda registrado de forma persistente en el sistema de ficheros.

---

# Enviar un trabajo a la cola

Para enviar un experimento a la cola se utiliza la orden:
```bash
gpuq submit --project PATH
```

donde `PATH` es el directorio que contiene el proyecto.

Por ejemplo:
```bash
gpuq submit --project ./experiment
```

El directorio del proyecto debe contener al menos un fichero `docker-compose` que describa cómo ejecutar el experimento.

Cuando se ejecuta esta orden:

- se genera un identificador único para el trabajo
- el trabajo se asocia al usuario que lo ha enviado
- se registra la fecha de creación
- el trabajo se guarda en la cola con estado **queued**

El trabajo permanecerá en ese estado hasta que el sistema encargado de ejecutar la cola lo inicie.

---

# Seleccionar el fichero docker-compose

Por defecto `gpuq` utilizará el fichero `docker-compose.yml`.  Si el proyecto contiene varios ficheros `docker-compose`, es posible seleccionar uno concreto utilizando la opción `--compose`.

Por ejemplo:

```bash
gpuq submit --project ./experiment --compose docker-compose.test.yml
```

Esto permite utilizar diferentes configuraciones dentro del mismo proyecto.

Por ejemplo:

- ejecutar el experimento principal
- ejecutar pruebas rápidas
- ejecutar configuraciones alternativas

---

# Añadir una descripción al trabajo

Es posible añadir una descripción opcional al trabajo utilizando la opción `--description`.

Por ejemplo:

```bash
gpuq submit --project ./experiment --description "baseline training"
```

La descripción es un texto libre que permite identificar el propósito del experimento.

---

# Qué ocurre al enviar un trabajo

Cuando se ejecuta `gpuq submit`, la herramienta **no ejecuta el experimento directamente**.

En su lugar:

1. valida que el directorio del proyecto existe
2. registra el trabajo en la cola
3. asigna un identificador único al trabajo
4. guarda los metadatos del trabajo en el sistema de ficheros

El trabajo queda registrado con estado `queued`. Un componente independiente del sistema se encargará posteriormente de ejecutar el trabajo.

---

# Identificador del trabajo

Cada trabajo recibe un identificador único con un formato similar a `job-1a2b3c4d`. Este identificador se utiliza para:

- consultar el estado del trabajo
- cancelar el trabajo
- identificar el experimento en la cola

---

# Qué hace gpuq y qué no hace

Es importante entender qué responsabilidades tiene `gpuq` y cuáles no.

`gpuq` se encarga de:

- registrar trabajos en una cola persistente
- asignar identificadores únicos a los trabajos
- mantener el estado de cada trabajo
- permitir listar y cancelar trabajos

Sin embargo, `gpuq` **no ejecuta directamente los contenedores**.  
La ejecución real es realizada por un componente independiente del sistema.

Además, `gpuq` **no elimina contenedores ni imágenes Docker**.

Cuando un trabajo termina, los contenedores creados por Docker permanecen en el sistema. Del mismo modo, las imágenes Docker construidas para ejecutar los experimentos tampoco se eliminan automáticamente.

La limpieza de contenedores o imágenes que ya no sean necesarios sigue siendo responsabilidad del usuario.

Las órdenes necesarias para gestionar contenedores e imágenes se describen en la sección sobre uso de Docker.

---

# Resumen

Las órdenes principales para enviar trabajos a la cola con gpuq son los siguientes:

| Orden                                                                | Descripción                                                                             |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `gpuq submit --project ./experiment`                                   | Envía un experimento a la cola utilizando el fichero `docker-compose.yml` del proyecto. |
| `gpuq submit --project ./experiment --compose docker-compose.test.yml` | Envía el experimento utilizando un fichero `docker-compose` específico.                 |
| `gpuq submit --project ./experiment --description "baseline training"` | Envía un experimento añadiendo una descripción para identificar el trabajo.             |

En todos los casos la orden registra el trabajo en la cola, genera un identificador único y deja el trabajo en estado queued hasta que el sistema encargado de ejecutar la cola lo inicie.

En la siguiente sección veremos cómo monitorizar los trabajos en la cola y consultar su estado.