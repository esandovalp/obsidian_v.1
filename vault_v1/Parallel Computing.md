 #8semester
**Repositorio de la clase:** https://github.com/octavio-gutierrez/computoparalelo2024a 
# ¿Qué es? 

La computación paralela es el proceso de descomponer problemas de gran envergadura en partes más pequeñas, independientes y a menudo similares, que pueden ser ejecutadas simultáneamente por varios procesadores que se comunican a través de una memoria compartida, y cuyos resultados se combinan al finalizar como parte de un algoritmo global.
# Objetivo:

El principal objetivo de la computación paralela es aumentar la potencia de cálculo disponible para acelerar el procesamiento de aplicaciones y la resolución de problemas.
# ¿Por qué es fácil/complicado trabajar en paralelo?

Se llega un punto en el que agregar más componentes no ayuda.
[[Debugging]] es complejo; se usan múltiples programas a la vez, se entrelazan y no es determinístico.

> **¿Por qué paralelizar?** 
 El chiste del computo paralelo es utilizar la [[Arquitectura de un CPU]] para poder obtener speed ups de manera eficiente, i.e. compartiendo los [[cpu threads]] pertinentemente, saber que partes de un programa paralelizar, etc. 

# Lenguajes orientados a cómputo paralelo 

 Entre los lenguajes de programación que usamos están [[julia]], [[c++]] y [[python]].

> Nunca hacer print en una paralelización, es pesado. Nunca hacer.
## Tipos de paralelización
- Un proceso con múltiples hilos.
- Múltiples procesos para múltiples hilos
- Los workers son los que nos interesan, son los que trabajan.
- Al agregar varios procesos, das tareas diferentes a cada uno de ellos. Esto en programas chicos no hace la diferencia.
- Hay muchos procesos por core
- Al matar la consola, se pierden los workers y procesos
- Para resultados más consistentes hay que paralelizar agregando más tareas.

## Datos random 

- Allocations son las reservas
- Entre más procesos más asignaciones de memoria hay
- [[Protocolo TCP]]
- Agregar más procesos también agrega más conexiones en TCP

## Aplicaciones del computo paralelo 

- Entrenamiento de redes profundas. Se necesita dar un avance en la energía ya que paralelizar ocupa muchísima energía (en este caso).

## Concurrencia vs Paralelismo

**Ejecución concurrente:** aquello que caracteriza a dos procesos en los que no sabes cuales se ejecuta primero o después. Ocurre en un solo lugar. Tiene que ver los tiempos y en donde se ejecuta
**Ejecución Paralela:** es un subconjunto de la concurrencia. Todo programa concurrente es paralelo, pero no todo paralelo es concurrente. Uso de recursos para acelerar. 
#### Cosas que dijo
- Se pueden agregar varios hilos a un proceso. 
- El tiempo que tiene un proceso se divide en los hilos
- Es bueno para tener múltiples clientes en una API, ya que esta es una base de datos
- CPU Threads = Kernel Threads
- La relación procesos-threads: muchos a pocos
- Con los procesos que llaman a sistemas los caches se van repoblando, es por eso que el print es muy lento. 
- Para un uso eficiente necesitas conocer la estructura de la computadora en la que estás trabajando  
- El CPU es ejecución generalizada, el GPU es ejecución especializada (es realmente un procesador)

