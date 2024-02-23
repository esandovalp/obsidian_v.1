#CS 
# What is C++?

- **A Powerful Programming Language:** C++ is a high-performance, general-purpose programming language famed for its speed and flexibility. It builds upon the foundations of the older C language.
- **Direct Hardware Control:** It excels in situations where you need a high degree of control over how your program utilizes the computer's hardware.
- **Widely Used:** C++ finds usage in a huge range of applications, including:
    - Operating systems
    - Game development
    - High-performance computing
    - Scientific simulations
    - Financial modeling

# Why is C++ used in parallel computing?

1. **Performance:** C++ is all about speed. Efficiently written C++ code can get very close to the theoretical maximum performance of your computer's hardware. This is critical for parallel computing, where squeezing every drop of power from multiple cores becomes vital.
2.  **Fine-Grained Control:** C++ provides low-level control over memory management and thread behavior. This lets programmers carefully optimize how various parts of their programs interact with multiple CPU cores and avoid situations where cores may sit idle.
3.  **Powerful Existing Libraries:** There's a wealth of well-established C++ libraries and tools devoted to parallel computing:
    - **OpenMP:** A popular API for adding parallel directives into C++ code for easy multithreading.
    - **Thread Building Blocks (TBB):** A framework to simplify complex parallel programming patterns.
    - **MPI (Message Passing Interface):** Allows programs running on multiple computers connected over a network to work together on large problems.
4. **Legacy and Community:** C++ has been around for decades. This means many of the fundamental algorithms and software used in parallel computing are already optimized and implemented in C++, alongside a large community of skilled developers.

## Challenges

- **Complexity:** Parallel programming, in general, adds significant complexity. C++ offers the tools to manage it, but this introduces a steeper learning curve compared to higher-level languages focused on parallelism.
- **Potential for Errors:** With so much control over multithreading, a programmer must be meticulous to avoid subtle errors like race conditions and deadlocks, which are notoriously difficult to debug.

# Is C++ the only option?

Absolutely not! Other great languages exist for parallel computing:
- **Rust:** Emphasizes safety and prevents many parallel programming errors at compile time.
- **Python:** Offers libraries like Dask and Ray for high-level parallel computing, often easier for beginners.
- **Java:** Also provides strong support for multithreading.
C++ excels in specific parallel computing contexts where raw performance and low-level control are paramount.

## Acaba intro empieza C++ OpenMP

- Tenemos que trabajar con la reserva dinámica del arreglo, para que se mande al heap (la RAM lo límita) y no al Stack.
	- El Heap va creciendo dinámicamente
# Apuntadores
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
# Arreglos dinámicos
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

# Código de clase con Open MP 
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

# MATRICES CPP
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

# Pasando argumentos en C++ 
- El ```argc``` es 1 porque el primero es el nombre del ejecutable 
# OpenMP
## Constructos para compartir trabajo

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
```cpp
#include <iostream>
#include <omp.h>
#include <vector>

using namespace std;

int main() {
    int N = 1000000000; 

    vector<float> a(N);
    vector<float> b(N);
    vector<float> c(N);

    // Serial Loop 
    double time2 = omp_get_wtime(); 
    for (int i = 0; i < N; ++i) {
        c[i] = a[i] + b[i];
    }
    double etime2 = omp_get_wtime();
    double ftime2 = etime2 - time2;
    std::cout << "Execution time serial loop: " << ftime2 << " seconds" << std::endl;
    cout << "\n";

    // Determine the maximum number of threads
    int max_threads = omp_get_max_threads();
    std::cout << "Maximum number of threads: " << max_threads << std::endl; // el maximo son 11 
    // testear haciendo 
    omp_set_num_threads(max_threads); // 1, 3, 6, 11 

	// chunksize probar con 1,2,4,6,8,16,32,64,...,1260
    // Parallel loop with dynamic thread control
    double start_time2 = omp_get_wtime();
    #pragma omp parallel for schedule(static, 160) 
    for (int i = 0; i < N; ++i) {
        int num_threads = omp_get_num_threads(); // Get threads inside parallel region
        c[i] = a[i] + b[i];
    }
    double end_time2 = omp_get_wtime();

    double elapsed_time2 = end_time2 - start_time2;
    std::cout << "Execution time parallel loop: " << elapsed_time2 << " seconds" << std::endl;

    return 0;
}
```
- Conforme aumentas los num_threads, aumentan el overhead, entonces puede haber un speed down. 
- Te conviene poner el ```chunk_size``` grande para repartir los pedazos del pastel más grande (para lo que estamos trabajando).
- Si vas a medir código serial, usa medición de tiempo serial 
- ```auto```: adopta el tipo que te va a regresar automaticamente, es de ```c++```
- ```shared(a,b,c, static_behavior```: lo que comparten todos los hilos.
- ```long long int``` cuando vamos a trabajar con vectores muy grandes 

- ```#pragma omp parallel for collapse(num_for)```: si son dos ```for``` los hace uno. 

## Bloque de código ejecutado exclusivamente por el hilo maestro
```cpp
#pragma omp master newline
	//structured_block
```
### Directiva sections 
Especifica que las secciones deben repartirse entre los hilos. Solo se permite un hilo por sección. Ejemplos: embeddings de texto, es una conversión de texto a números. 
```cpp
#pragma omp sections newline
{
	#pragma omp section newline
	// structured_block
	#pragma omp section newline 
	// structured_block
}
```

### Directiva SINGLE

Bloque de código ejecutado exclusivamente por un hilo 

### Directiva BARRIER

Obliga a todos los hilos a esperarse los unos a los otros 

### Directiva Critical y Atomic 

En la critical puedes meter más instrucciones, atomic es mucho más peligrosa, pero ambas son similares. 

## Código de directivas 

```cpp
#include <iostream>
#include <omp.h>

int main() {
  const int num_threads = 4;
  const long long int vector_size = 1000000;
  int chunk_size = vector_size / num_threads;
  int *a = new int[vector_size];
  int *b = new int[vector_size];
  int *c = new int[vector_size];
  long long int i = 0;
  int thread_id = 0;
  omp_set_num_threads(num_threads)
#pragma omp parallel private(i, thread_id)
  {
    thread_id = omp_get_thread_num();
#pragma omp critical
    std::cout << thread_id << " esperando.. \n";
    // #pragma omp barrier
    std::cout << "hilo " << thread_id << " avanzando.. \n";
  }

  std::cout << "hello";
  return 0;
}

```
Compilación
```bash
g++-13 -fopenmp -fdiagnostics-color=always -g /Users/esandovalp/Documents/vscodeFiles/compuParalelo/cpp/directivas.cpp -o /Users/esandovalp/Documents/vscodeFiles/compuParalelo/cpp/directivas
```
Correrlo
```bash
./directivas
```
**Sections_**
```cpp
```

## Cláusula REDUCTION

- }

Ahora procedemos con [[MPI]]