# Para qué funcionan

- Ajustan y consultan el número de hilos 
- Consultar el id único de un hilo 
- Verifican si se encuentran en una región paralela 
- Manejo de candados 
- Consultan el tiempo de reloj de pared

## Ejemplo 

```cpp
#include <omp.h>
int omp_get_num_threads(void)
```
