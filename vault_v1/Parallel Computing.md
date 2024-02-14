 #8semester
## ¿Qué es? 
La computación paralela es el proceso de descomponer problemas de gran envergadura en partes más pequeñas, independientes y a menudo similares, que pueden ser ejecutadas simultáneamente por varios procesadores que se comunican a través de una memoria compartida, y cuyos resultados se combinan al finalizar como parte de un algoritmo global.
## Objetivo:
El principal objetivo de la computación paralela es aumentar la potencia de cálculo disponible para acelerar el procesamiento de aplicaciones y la resolución de problemas.
## ¿Por qué es fácil/complicado trabajar en paralelo?
Se llega un punto en el que agregar más componentes no ayuda.
[[Debugging]] es complejo; se usan múltiples programas a la vez, se entrelazan y no es determinístico.
## [[Arquitectura de un CPU]]

**Cache**:
- El L(level)1 de un CPU es como nuestra memoria corta
- L2 le cabe más
- L3 le cabe aún más. Se comparte entre varios cores
- No es bueno forzar ordenes (de como corran los programas)
- El cache son los levels
## Lenguajes de programación orientados a la Paralelización

### [[Julia]]

- Una multiplicación no es atómica, es decir, no se puede dividir. En ensamblador se traduce en múltiples instrucciones.
- El problema que surge con esto: no es determinístico, es estocástico. Por lo que de repente el programa va a fallar. Hay muy pocas instrucciones atómicas.
- Entré más workers es más rápido.
- Si haces ‘nprocs()’ agrega n-1 workers, sólo en la primera vez que agregas procesos, no agrega el worker
- Nos interesa el nworkers()
#### Primer programa de paralelización
_slide 31 de Introduccion y arquitecturas paralelas.pdf_
- Nunca hacer print en una paralelización, es pesado. Nunca hacer.
- Medir tiempo en julia:  
    ```julia
    @time azar()
    ```
#### Tipos de paralelización
- Un proceso con múltiples hilos.
- Múltiples procesos para múltiples hilos
- Los workers son los que nos interesan, son los que trabajan.
- Al agregar varios procesos, das tareas diferentes a cada uno de ellos. Esto en programas chicos no hace la diferencia.
¿Cuál es la relación entre el número de procesos y cores de un cpu?
RE: Muchas a uno
- Al matar la consola, se pierden los workers y procesos
- Para resultados más consistentes hay que paralelizar agregando más tareas.
- La diferencia entre azarparalela y azar es que azar sin paralelizar tarda el doble
**Resultados**
- allocations son las reservas
- entre más procesos más asignaciones de memoria hay
- el protocolo tcp:
- agregar más procesos también agrega más conexiones en TCP
**Aplicaciones**
- Entrenamiento de redes profundas. Se necesita dar un avance en la energía ya que paralelizar ocupa muchísima energía (en este caso).
## Clase 4
- ChatGPT no genera buen código paralelo 
- Entre más cores agregas y vas bajando el tiempo, se asume que estás haciendo un uso eficiente
#### Concurrencia vs Paralelismo
**Ejecución concurrente:** aquello que caracteriza a dos procesos en los que no sabes cuales se ejecuta primero o después. Ocurre en un solo lugar. Tiene que ver los tiempos y en donde se ejecuta
**Ejecución Paralela:** es un subconjunto de la concurrencia. Todo programa concurrente es paralelo, pero no todo paralelo es concurrente. Uso de recursos para acelerar. 
#### Cosas que dijo
- Se pueden agregar varios hilos a un proceso. 
- El tiempo que tiene un proceso se divide en los hilos
- Es bueno para tener múltiples clientes en una API, ya que esta es una base de datos
- CPU Threads = Kernel Threads
- La relación procesos-threads muchos a pocos
- Con los procesos que llaman a sistemas los caches se van repoblando, es por eso que el print es muy lento. 
- Para un uso eficiente necesitas conocer la estructura de la computadora en la que estás trabajando  
- El CPU es ejecución generalizada, el GPU es ejecución especializada (es realmente un procesador)
## 30 de Enero
#### Speed Ups
- Línea base: piensas que si metes el doble de cores esperas tener el doble de speed up
- $$\text{Speed up}_\text{linea base}=\frac{1}{1-f+\frac{f}{N}}$$
- Tarea altamente paralelizable: ¿cuantas palabras tiene un documento? *ver como se hace*
- Comparar teniendo otro referencias de otro sistema
- Ya no se cumple la ley de Moore
	- Cuellos de botella:
		- Al aumentar la cantidad de transistores aumenta la energía (por el calor que genera)
		- El acceso a la RAM es muy pesado
- Los transistores son la base de la computación 
	- Son como un circuito, la corriente abre o cierra el circuito 
	- Con dos transistores se vuelven en puertas (AND, OR, XOR, NOT, etc...)
	- Compuerta principal/base: NOT 
	- *ver vídeo de transistores*
	- El reloj lanza los pulsos que abren las puertas
	- Muchos GHz sobrecalientan la compu. 
		- $$ 3.2 \text{ Ghz}=3200 \text{ millones de ciclos } \times \text{ segundos}$$
	- Paralelismo en la compu: *buscar pipelines en internet o en presentación*
## 1 de Febrero
- Las computadoras seriales son las de un único propósito
- Si hay threads que actúan sobre un mismo programa ocasiona que puedan haber distintos resultados y se convierta en no determinístico.
- Ejemplo de multiples instrucciones un solo flujo de datos: cuando había varios algoritmos para descifrar un sólo código (como en la guerra fría).
- Sincronía: existen ciertos periodos para sincronizarse, hay procesos que están esperando, en específico los hilos.
- No determinismo: múltiples hilos lo puede ocasionar. No controlamos como se comportan los hilos.
- Por procesos hay al menos un hilo. Ahora trabajaremos con hilos (también se le conocen como procesos ligeros)
    ```julia
    using .Threads
    ```
    - Vamos a tener múltiples hilos por proceso
    ```julia
    using .Threads
    
    function suma(n)
        arreglo = Int[]
        @threads for i in 1:n
            push!(arreglo, i)
            println("Soy el hilo ", Threads.threadid(), " y estoy agregando ", i)
        end
        print(arreglo)
        return arreglo
    end
    ```
    
    - El que hilo va a ejecutar que se hace de manera estócastica.
    - Cada uno corre su propio flujo de instrucciones
    
    ```julia
    using .Threads
    
    function suma(n)
        arreglo = Int[]
        lk = ReentrantLock()
        @threads for i in 1:n
            lock(lk) do 
                push!(arreglo, i)
            end
            #println("Soy el hilo ", Threads.threadid(), " y estoy agregando ", i)
        end
        #print(arreglo)
        return arreglo
    end
    ```
    - Así se evita que cada vez que se corre
    ```julia
    sum(suma(1000))
    ```
    - Salga el mismo resultado.
- El verdadero cuello de botella es el acceso a memoria
- Ir a L1 tarda 4 ciclos, una operación aritmética es 1 ciclo.
- Mejor caso a la DRAM son ~248

- Existe un número adecuado de hilos por programa, arquitectura, etc

## Clase 6 de Febrero

### Código de prueba de Julia 

```julia
using Base.Threads

  
function disminucion_threaded_no_locks(n)

	valor = Atomic{Int}(10000000)	
	@threads for i in 1:n
	atomic_sub!(valor, 1)
	end
	return valor[]
end

function disminucion_threaded_with_locks(n)
	valor = Atomic{Int}(10000000)
	lock = ReentrantLock()
	@threads for i in 1:n
	lock!(lock) # no está la versión de Julia que tiene lock :( en Dev Containers de VSCode
	atomic_sub!(valor, 1)
	unlock!(lock)
	end
	return valor[]
end

  

function disminucion_threaded_modified_operation(n)
	valor = Atomic{Int}(10000000)
	@threads for i in 1:n
		valor[] = 10000000 - i
	end
	return valor[]
end
```

Correlo en la shell de Julia 
```julia
using BenchmarkTools

include("prueba.jl")  # Make sure this path is correct

n = 10000000

time_no_locks = @benchmark disminucion_threaded_no_locks($n)
time_with_locks = @benchmark disminucion_threaded_with_locks($n)
time_modified_op = @benchmark disminucion_threaded_modified_operation($n)

println("Time without locks: ", time_no_locks)
println("Time with locks: ", time_with_locks)
println("Time with modified operation: ", time_modified_op)

```

- La función `disminucion_threaded_no_locks` tarda unos 226,965 milisegundos. Esta versión no es segura para hilos, pero es más rápida debido a la falta de sobrecarga de sincronización.
- La función `disminucion_threaded_modified_operation`, que evita la necesidad de bloqueos utilizando un enfoque diferente para decrementar `valor`, es significativamente más rápida con unos 10,921 milisegundos. Esto es de esperar, ya que elimina la necesidad de operaciones atómicas en una variable compartida, que puede ser un cuello de botella en entornos multihilo.
- No corrió with locks por la versión que Dev Containers maneja de Julia 
- Sacar la conclusión con Locks, 
## Acaba intro empieza C++ OpenMP

- Tenemos que trabajar con la reserva dinámica del arreglo, para que se mande al heap (la RAM lo límita) y no al Stack.
	- El Heap va creciendo dinámicamente
##### apuntadores
```c++
#include <iostream>
int main(){
    int valor1 = 1;
    int valor2 = 2;

    int* apuntador = &valor1;   
    *apuntador = 10;
    apuntador = &valor2;
    *apuntador = 20;

    std::cout << "Donde vive el valor 1 " << &valor1 << "\n";
    std::cout << "Donde vive el valor 2 " << &valor2 << "\n";
    std::cout << "El valor 1 " << valor1 << "\n";
    std::cout << "El valor 2 " << valor2 << "\n";

//    std::cout << "Donde vive el apuntado " << &apuntador << "\n";
//    std::cout << "Imprimiendo el apuntador " << apuntador << "\n";
//    std::cout << "Imprimiendo el apuntador con el puente" << *apuntador << "\n";

    return 0;
}
```
- ```int*``` : Define la variable que guarda la dirección. Este apuntador también tiene una dirección única. 
- ```&valor1```: Guarda la dirección
- Un apuntador es una variable que almacena una dirección.
##### Arreglos dinámicos
```c++
#include <iostream>
using namespace std;

int main() {    
    int a[10];
    int* b {new int[10]{11, 12, 13, 14, 15, 
                        16, 17, 18, 19, 20}};
    int* c = new int[10]; //valores arbitrarios
    int* d;
    d = new int[10];                        
  
    for (int i = 0; i < 10; i++) {
        cout << "a " << a[i] << endl;
        cout << "b " << b[i] << endl;
        cout << "c " << c[i] << endl;
        cout << "d " << d[i] << endl;
    } 
  
    //delete[] a; Evitar memory leaks
    delete[] b;
    delete[] c;
    delete[] d; 
  
    cout << "Hello";
    return 0;
}
```

#### Código de clase con Open MP 
*lo solucione con esta página:* https://www.bilibili.com/read/cv15365733/
```c++
#include <iostream>
#include <omp.h>
using namespace std;

int main() {
	int nthreads;
	int thread_id;
	
	omp_set_num_threads(4);
	#pragma omp parallel private(thread_id)
	{
		thread_id = omp_get_thread_num();
		cout << " Hola soy el hilo " << thread_id << "\n";	
		// solo lo ejecuta el hilo maestro
		if (thread_id == 0) {
			nthreads = omp_get_num_threads();
			cout << "Hay " << nthreads << " hilos \n";
		}
	}
	return 0;
}
```
Para compilarlo: 
```bash
g++-13 -fopenmp -fdiagnostics-color=always -g /Users/esandovalp/Documents/octavoCODE/cpp/paralelo.cpp -o /Users/esandovalp/Documents/octavoCODE/cpp/paralelo
```
Para correrlo, ir primero al directorio donde se hizo el archivo:
```bash
./paralelo
```
### código de prueba con MPI 
```c++
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
#### Compilarlo 
```bash
mpic++ -o show.out test.cpp
```
#### correrlo
Se puede cambiar el 4, no se muy bien que sea, pueden ser los threads
```bash
mpirun -n 4 show.out
```

# Clase 13 02 

#### MATRICES CPP
```cpp
#include <iostream>
using namespace std;
// este código se puede correr simplemente dandole a run

int main() {
	int ren = 2;
	int col = 4;
	
	//int matriz[ren][col]; está bien pero sólo para arreglos chicos
	// int** matriz{new int*[ren]}; apuntador al primer renglon de la matriz
	// Reservamos memoria para la matriz dinamicamente
	int** matriz = new int*[ren]; //notación más java, arreglo de apuntadores
	for (int i = 0; i < ren; i++) {
		matriz[i] = new int[col];
	}

	// Codigo de procesamiento de la matriz	
	for (int i = 0; i < ren; i++) {
		for (int j = 0; j < col; j++) {
			cout << matriz[i][j] << " ";
		}
	
		cout << "\n";
	}
	cout << "\n";

	cout << *matriz << "\n";	
	for (int i = 0; i < ren; i++) {
		cout << matriz[i] << "\n";
	
		for (int j = 0; j < col; j++) {
		cout << &matriz[i][j] << " ";
		}
		cout << "\n";
	}
	
	cout << "\n";
	// Evitar memory leaks
	// Libernado la memoria reservada dinamicamente
	for (int i = 0; i < ren; i++) {
		delete [] matriz[i];
	}

	delete[] matriz; // si no haces nada de esto no te va a marcar error pero es buena práctica

	cout << "hello world\n";
	return 0;
}
```

#### Pasando argumentos en C++ 

- El ```argc``` es 1 porque el primero es el nombre del ejecutable 

#### OpenMP
##### Constructos para compartir trabajo

- ```#pragma omp parallel for```: Es el que se recomienda usar porque se puede usar casi en cualquier lugar en donde ves un ```for```. Escenario donde no, depender de una entrada, hasta encontrar un carácter específico en un archivo. Distribuye los threads uniformemente. La etiqueta se pone antes del ```for```. 
	- ```q[i] + b[i]```: Esto se mantiene constante 
		```cpp
#include <iostream>
#include <omp.h>
#include <vector>
using namespace std;

int main() {
	int N = 1000000;
	vector<float> a(N);
	vector<float> b(N);
	vector<float> c(N);
	
	// Serial Loop 1
	double time1 = omp_get_wtime();
	for (int i = 0; i < N; ++i) {
		a[i] = b[i] = i * 1.0;
	}
	double etime1 = omp_get_wtime();
	
	// Serial Loop 2
	double time2 = omp_get_wtime();
	for (int i = 0; i < N; ++i) {
		c[i] = a[i] + b[i];
	}
	double etime2 = omp_get_wtime();
	// Printing Results
	double ftime1 = etime1 - time1;

	std::cout << "Execution time first serial loop: " << ftime1 << " seconds" << std::endl;

	double ftime2 = etime2 - time2;

	std::cout << "Execution time second serial loop: " << ftime2 << " seconds" << std::endl;

	cout << "\n";

	//omp_set_num_threads(4);
	double start_time1 = omp_get_wtime();
	#pragma omp parallel for
	for (int i = 0; i < N; ++i) {
		a[i] = b[i] = i * 1.0;
	}
	double end_time1 = omp_get_wtime();

	double start_time2 = omp_get_wtime();
	#pragma omp parallel for
	for (int i = 0; i < N; ++i) {
		c[i] = a[i] + b[i];
	}
	double end_time2 = omp_get_wtime();

	double elapsed_time1 = end_time1 - start_time1;
	
	std::cout << "Execution time first parallel loop: " << elapsed_time1 << " seconds" << std::endl;

  

	double elapsed_time2 = end_time2 - start_time2;
	std::cout << "Execution time second parallel loop: " << elapsed_time2 << " seconds" << std::endl;
	return 0;
}
```
		Resultados
		```bash
Execution time first serial loop: 0.005138 seconds
Execution time second serial loop: 0.005472 seconds
Execution time first parallel loop: 0.0008 seconds
Execution time second parallel loop: 0.000781 seconds
		```
- ```#pragma omp for schedule(dynamic, chunk_size)```: el ```chunk_size``` es el tamaño de la carga, se usa cuando utilizamos una operación dinámica. 
	- ```clustering[region]```: cuando sabes que algún hilo terminará más rápido. 
- ```#pragma omp for schedule(static, chunk_size)```: 
- ```#pragma omp parallel for collapse(num_for)```: 