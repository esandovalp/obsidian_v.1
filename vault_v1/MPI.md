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

# Primes with MPI

```cpp
#include <iostream>
#include <mpi.h>
#include <cmath>
#include <vector>
#include <chrono>

// Function to check if a number is prime
bool isPrime(int n) {
if (n <= 1) return false;
if (n <= 3) return true;
if (n % 2 == 0 || n % 3 == 0) return false;

for (int i = 5; i * i <= n; i += 6) {
	if (n % i == 0 || n % (i + 2) == 0) return false;
}
return true;
}

int main(int argc, char **argv) {
	// Initialize the MPI environment
	MPI_Init(&argc, &argv);
	// Get the number of processes
	int world_size;
	MPI_Comm_size(MPI_COMM_WORLD, &world_size);

	// Get the rank of the process
	int world_rank;
	MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
	// Broadcast n to all processes
	int n;
	
	if (world_rank == 0) {
	// Assuming you pass the exponent as the first argument to the program
	n = (argc > 1) ? std::atoi(argv[1]) : 2; // Default to 10^2 if no argument is provided
	}
	MPI_Bcast(&n, 1, MPI_INT, 0, MPI_COMM_WORLD);

	// Calculate the range for each process
	int range = std::pow(10, n);
	int local_count = 0;
	int global_count;

	auto start = std::chrono::high_resolution_clock::now();
	// Each process checks a subset of the range for primes
	for (int i = world_rank + 2; i <= range; i += world_size) {
		if (isPrime(i)) {
		local_count++;
		}
	}

	// Reduce all local counts into the global count
	MPI_Reduce(&local_count, &global_count, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);  
	auto finish = std::chrono::high_resolution_clock::now();
	std::chrono::duration<double> elapsed = finish - start;
	
	// Process 0 prints the results
	if (world_rank == 0) {
		std::cout << "Range: [2, " << range << "]" << std::endl;
		std::cout << "Prime numbers in range: " << global_count << std::endl;
	std::cout << "Time taken: " << elapsed.count() << " seconds" << std::endl;
}

	// Finalize the MPI environment.
	MPI_Finalize();
	return 0;
}
```

## compile 
```bash
mpicxx -o mpi_prime mpi_prime.cpp
```

## output

```bash
mpiexec -np 4 ./mpi_prime 6
Range: [2, 1000000]
Prime numbers in range: 78498
Time taken: 0.0130217 seconds
```

## comparación (tiempo en segundos)

| n   | serial     | mpi         | speed up | procesos mpi |
| --- | ---------- | ----------- | -------- | ------------ |
| 1   | 6.25e-07   | 3.125e-05   | x2?      | 4            |
| 2   | 9.16e-07   | 3.6958e-05  | x2.5?    | 4            |
| 3   | 7.625e-06  | 2.5667e-05  |          | 4            |
| 4   | 0.000205   | 7.025e-05   |          | 4            |
| 5   | 0.00365804 | 0.000634459 |          | 4            |
| 6   | 0.0360119  | 0.0110806   |          | 4            |
| 7   | 0.405107   | 0.228191    |          | 4            |
| 8   | 9.43592    | 5.60618     |          | 4            |
| 9   |            | 149.207     |          | 4            |
