#cs #8semester 
# Respuesta de Gemini sobre OpenMP 

## What is OpenMP?
*Open specifications for Multi-Processing*

- Single Program, Multiple Data (subcategoría de MIMD)
- Aplicaciones paralelas con memoria compartida
- Control total- paralelismo explícito (no automático)
- Mecanismos de sincronización
- Número dinámico de hilos
- Cada hilo puede tener variables compartidas y locales
- **Compiler Directives, Library Routines, and Environment Variables:** The core of OpenMP consists of:
    - **[[OpenMP Directives]]:** Special instructions you add to your code to tell the compiler how to parallelize sections.
    - **[[OpenMP Libraries]]:** Functions that help manage threads and parallel execution.
    - **[[Environment Variables]]:** Settings that influence OpenMP's behavior during program execution.

# Funcionamiento de los hilos en OpenMP 

Los **hilos** tienen potencial de ejecutar el mismo código, sin embargo, cada hilo puede
acceder diferentes datos y atravesar diferentes rutas de ejecución.

**Metas:**
- Estandarización
- Programación paralela simple
- “Portable”

# Clase 
- Tenemos que trabajar con la reserva dinámica del arreglo, para que se mande al heap (la RAM lo limita) y no al Stack.
	- El Heap va creciendo dinámicamente

# Cómo correr un programa en C++ con OpenMP 
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
# Constructos para compartir trabajo

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

**Problemas que pueden ocurrir con esta solución:**
- Dependiendo del tamaño de los for's anidados, no todos los hilos pueden llegar a utilizarse. La solución para esto es utilizar: ```#pragma omp parallel for collapse(num_for)```: si son dos ```for``` los hace uno. 
## Floyd - Marshall 
- El algoritmo puede usar tres ```for```anidados, por lo que para eficientar el algoritmo, usamos estos "pragmas".
## Más constructos 

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

Ahora procedemos con [[MPI]]

[[Proyecto]]