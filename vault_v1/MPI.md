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
mpic++ -o mpi_hello_world.out mpi_hello_world.cpp
```
## Correrlo

Se puede cambiar el 4, no se muy bien que sea, pueden ser los threads
```bash
mpirun -n 4 show.out
```

# Directivas

- ```MPI_Send```: es el que manda
- ```MPI_Rec```: define funciones call back, que no avanza hasta que recibe el mensaje. Comunicación no bloqueante. 
- ```MPI_Comm_size```: es lo que indica con que grupo se va a comunicar, generalmente se usa como ```MPI_Comm_size(MPI_COMM_WORLD, &num_processes);```.
	- ```MPI_COMM_WORLD```: es comunicarse con todos, es el que usaremos más.
- ```MPI_Comm_rank```: hace referencia al identificador. Se usa como ```MPI_Comm_rank(MPI_COMM_WORLD, &process_id);```
- ```MPI_Recv```: es el que recibe

# Rutinas de comunicación colectiva

## Clase sender

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
	int destination = 0;
	int mensaje[5] = {11,12,13,14,15};
	int tag = 101; // le puedes poner lo que sea para identificar el mensaje
	MPI_Init(&argc, &argv);
	MPI_Comm_size(MPI_COMM_WORLD, &num_processes);
	MPI_Comm_rank(MPI_COMM_WORLD, &process_id);
	MPI_Get_processor_name(host_name, &name_length);
	cout << "Hi from process " << process_id << " on " << host_name << "\n";
	
	MPI_Send(&mensaje, 5, MPI_INT, destination, tag, MPI_COMM_WORLD);
	cout << "Envie un mensaje \n";
	//cout << "MASTER: I have sent a message to process " << destination << "\n";
	MPI_Finalize();
	return 0;
}
```

## Clase receiver

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
int tag = 101; //tiene que ser el mismo que en sender
int source = 1;
int mensaje[5] = {0,0,0,0,0};

MPI_Status status;
MPI_Init(&argc, &argv);
MPI_Comm_size(MPI_COMM_WORLD, &num_processes);
MPI_Comm_rank(MPI_COMM_WORLD, &process_id);
MPI_Get_processor_name(host_name, &name_length);

cout << "Hi from processos " << process_id << " on " << host_name << "\n";
MPI_Recv(&mensaje, 5, MPI_INT, source, tag, MPI_COMM_WORLD, &status);
//Son bloqueantes, no lo manda hasta que recibe, espera a que falle (A que no este vivo el proceso) o hasta que recibe.
cout << "Recibi un mensaje de " << status.MPI_SOURCE << "\n";
for (int i = 0; i < 5; i++) {
	cout << "valor " << i << " ==  " << mensaje[i] << "\n";
}

MPI_Finalize();
return 0;
}
```

```bash
mpiexec -n 1 receiver.out : -n 1 sender.out
```

*Esto lo haremos solo una vez, generalmente los mensajes se mandan en un mismo archivo*

![[Pasted image 20240229190608.png]]
- Se comunica con la mitad de procesos, por eso es /2.
- Los identificadores son los que te dicen quien acaba ejecutando a quien.

# Ejercicio MPI roles A y roles B

```cpp
//solucion
```
