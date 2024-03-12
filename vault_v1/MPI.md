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

# Ejercicio MPI comunicación no bloqueante

```cpp
// Non-blocking communication
#include <mpi.h>
#include <iostream>
#include <vector>

int main(int argc, char *argv[]) {

MPI_Init(&argc, &argv);
int num_procs, rank;
MPI_Comm_size(MPI_COMM_WORLD, &num_procs);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);

int left = rank - 1;
int right = rank + 1;
if (rank == 0) left = num_procs - 1;
if (rank == num_procs - 1) right = 0;

int send_to_left = rank; // Data sent to the left neighbor
int send_to_right = rank; // Data sent to the right neighbor
int recv_from_left, recv_from_right;
MPI_Request requests[4];

// Send to right and receive from left
MPI_Isend(&send_to_right, 1, MPI_INT, right, rank, MPI_COMM_WORLD, &requests[0]);
MPI_Irecv(&recv_from_left, 1, MPI_INT, left, left, MPI_COMM_WORLD, &requests[1]);

// Send to left and receive from right
MPI_Isend(&send_to_left, 1, MPI_INT, left, rank, MPI_COMM_WORLD, &requests[2]);
MPI_Irecv(&recv_from_right, 1, MPI_INT, right, right, MPI_COMM_WORLD, &requests[3]);

// Wait for all non-blocking operations to complete
MPI_Waitall(4, requests, MPI_STATUSES_IGNORE);
// Output the results
std::cout << "Process " << rank << " received from left (" << left << "): "
<< recv_from_left << " and from right (" << right << "): " << recv_from_right << std::endl;

MPI_Finalize();
return 0;
}
```

### Output

```bash
λ ~ mpirun -n N non_blocking_communication

Process 9 received from left (8): 8 and from right (10): 10
Process 8 received from left (7): 7 and from right (9): 9
Process 7 received from left (6): 6 and from right (8): 8
Process 2 received from left (1): 1 and from right (3): 3
Process 6 received from left (5): 5 and from right (7): 7
Process 10 received from left (9): 9 and from right (0): 0
Process 0 received from left (10): 10 and from right (1): 1
Process 1 received from left (0): 0 and from right (2): 2
Process 3 received from left (2): 2 and from right (4): 4
Process 5 received from left (4): 4 and from right (6): 6
Process 4 received from left (3): 3 and from right (5): 5
```

# ejercicio clase jueves 7 de marzo

