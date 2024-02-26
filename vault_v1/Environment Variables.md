# Para qué sirven 

- Ajustan el número de hilos 
- Especifican cómo se dividen las iteraciones entre los hilos 
- Vinculan hilos a procesadores 
- Controlan los niveles de anidamiento de paralelismo 
- Deshabilitan o habilitan hilos dinámicos
- Ajustan el tamaño del Stack
## Ejemplo

```export OMP_NUM_THREADS=8```

# Tamaño del stack en macOS 

>  The stack size of a macOS main threads is **8MB** and 512KB of a secondary thread (on iOS the main thread has a stack size of 1MB).

# Definir el número de hilos 

```omp_set_num_threads()```

