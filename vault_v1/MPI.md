#8semester #CS 
*Message passing interface*

- Fue diseñado *originalmente* para memoria compartida.
- Manda mensajes de la computadora a la misma computadora, pero a otro proceso. 
# Por qué usar MPI 

- Rendimiento 
- Estandarización/Portabilidad
- Funcionalidad
# Código de prueba con MPI 

```cpp
#include <iostream>
#include "mpi.h"

using namespace std;

int main(int argc, char * argv[]){
	int my_rank, p, n;
	
	MPI_Init(&argc, &argv);
	MPI_Comm_size(MPI_COMM_WORLD, &p);
	MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);
	
	printf("size: %d rank: %d\n", p, my_rank);
	MPI_Finalize();
}
```
## Compilarlo 

```bash
mpic++ -o show.out test.cpp
```
## Correrlo

Se puede cambiar el 4, no se muy bien que sea, pueden ser los threads
```bash
mpirun -n 4 show.out
```

# Directivas

- ```MPI_Send```: 
- ```MPI_Rec```: define funciones call back, que no avanza hasta que recibe el mensaje. Comunicación no bloqueante. 