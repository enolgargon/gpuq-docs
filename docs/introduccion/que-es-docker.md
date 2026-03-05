# ¿Qué es Docker?

Docker es una herramienta que permite ejecutar programas dentro de **entornos aislados llamados contenedores**.

Un contenedor incluye todo lo necesario para ejecutar un programa:

- el sistema operativo base
- las librerías necesarias
- las dependencias de Python
- el propio código del experimento

Esto permite ejecutar un experimento **siempre en el mismo entorno**, independientemente del sistema donde se ejecute.

---

## El problema que intenta resolver

En experimentación con *machine learning* es muy común encontrarse con problemas como estos:

- un experimento funciona en tu ordenador pero falla en el servidor
- distintas versiones de Python o de una librería provocan errores
- instalar dependencias rompe otros proyectos
- diferentes usuarios necesitan entornos distintos

Por ejemplo, un experimento puede requerir python 3.10 y pytorch 2.1, mientras que otro puede necesitar python 3.8 y tensorflow 2.10.



Instalar todo esto en el mismo sistema suele generar conflictos.

---

## La solución: contenedores

Docker permite encapsular cada experimento en su propio entorno.

Ese entorno se describe en un fichero llamado **Dockerfile**, que define:

- qué imagen base se utiliza
- qué dependencias se instalan
- qué orden ejecuta el experimento

Cuando se ejecuta el experimento, Docker crea un **contenedor** con ese entorno.

De esta forma:

- cada experimento tiene sus propias dependencias
- no hay conflictos entre proyectos
- el experimento se puede reproducir fácilmente

---

## Ventajas de usar Docker en experimentación

El uso de Docker en experimentos tiene varias ventajas importantes.

### Reproducibilidad

El entorno del experimento queda completamente definido.

Si otra persona ejecuta el mismo contenedor, obtendrá **exactamente el mismo entorno**.

---

### Aislamiento

Cada experimento se ejecuta en su propio contenedor, por lo que:

- no afecta al sistema
- no rompe otros experimentos
- no requiere instalar librerías globalmente

---

### Portabilidad

Un contenedor Docker puede ejecutarse en cualquier sistema que tenga Docker instalado.

Esto permite mover experimentos fácilmente entre:

- ordenadores personales
- servidores de investigación
- infraestructuras de computación

---

## Docker en este servidor

En este servidor utilizamos Docker para ejecutar todos los experimentos.

Cada experimento se define mediante:

- un **Dockerfile** que describe el entorno
- un **docker-compose.yml** que describe cómo se ejecuta

Estos ficheros permiten definir completamente el experimento.

Sin embargo, aún queda un problema.

Si varios usuarios ejecutan contenedores que utilizan GPU al mismo tiempo, pueden producirse conflictos.

Para resolver esto utilizamos **gpuq**, una herramienta que permite **encolar experimentos y ejecutarlos de forma controlada**.