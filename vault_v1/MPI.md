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
#include <mpi.h>
#include <iostream>
using namespace std;

int main (int argc, char *argv[]) {

	const int MASTER = 0;
	int num_processes = 0;
	int process_id = 0;
	int name_length = 0;
	char host_name[MPI_MAX_PROCESSOR_NAME];
	
	MPI_Init(&argc, &argv);
	MPI_Comm_size(MPI_COMM_WORLD, &num_processes);
	MPI_Comm_rank(MPI_COMM_WORLD, &process_id);
	MPI_Get_processor_name(host_name, &name_length);
	
	cout << "Hi from processos " << process_id << " on " << host_name << "\n";
	
	if (process_id == MASTER)
		cout << "MASTER: The number of MPI processes is " << num_processes << "\n";
		
	MPI_Finalize();
	  
	return 0;
}
```
## Compilarlo 

```bash
mpic++ -o show.out mpi_hello_world.cpp
```
## Correrlo

Se puede cambiar el 4, no se muy bien que sea, pueden ser los threads
```bash
mpirun -n 4 show.out
```

# Directivas

- ```MPI_Send```: 
- ```MPI_Rec```: define funciones call back, que no avanza hasta que recibe el mensaje. Comunicación no bloqueante. 
- ```MPI_Comm_size```: es lo que indica con que grupo se va a comunicar, generalmente se usa como ```MPI_Comm_size(MPI_COMM_WORLD, &num_processes);```.
	- ```MPI_COMM_WORLD```: es comunicarse con todos
- ```MPI_Comm_rank```: hace referencia al identificador. Se usa como ```MPI_Comm_rank(MPI_COMM_WORLD, &process_id);```
- 