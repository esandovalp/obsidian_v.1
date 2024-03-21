#cs #8semester 
# Qué son y cómo son 

- Generan una región paralela 
- Dividen el código entre hilos
- Distribuyen las iteraciones entre los hilos 
- Serializar secciones de código: **preguntar**
- Sincronización de trabajo entre hilos 
## Formato

```#pragma omp constructo [cláusula]```

# Directiva sections 

Especifica que las secciones deben repartirse entre los hilos. Solo se permite un hilo por sección. 
**Ejemplos**: embeddings de texto, es una conversión de texto a números. 

```cpp
#pragma omp sections newline
{
	#pragma omp section newline
	// structured_block
	
	#pragma omp section newline 
	// structured_block
}
```

Al final de las secciones hay una barrera implícita a no ser que utilice la cláusula ```nowait```. **Preguntar**
# Directiva SINGLE

Bloque de código ejecutado exclusivamente por un hilo 
```cpp
#pragma omp single newline
	//structured_block
```
# Directiva BARRIER

Obliga a todos los hilos a esperarse los unos a los otros. Esta es la que evita condiciones de carrera
```cpp
#pragma omp barrier newline
```
# Directiva ATOMIC y CRITICAL

**Atomic:** la expresión se ejecuta como si fuera una sola instrucción a nivel máquina.
```cpp
#pragma omp atomic newline
	//statement_expression
```

**Critical:** región crítica
```cpp
#pragma omp critical (nombre) newline
	//structured_block
```
El ```(nombre)``` es muy importante ya que es el candado.
- Regiones con el mismo nombre son mutuamente excluyentes, es decir, no ocurren al mismo tiempo.
- Para tener regiones que no sean mutuamente excluyentes se le debe poner un nombre diferente a cada región. 

**Ejemplo de uso con otras directivas**
```cpp
#pragma omp parallel shared(x)
{
	#pragma omp critical
	x = x + 1;
}
```

En la critical puedes meter más instrucciones, atomic es mucho más peligrosa, pero ambas son similares. 

# Cláusula REDUCTION

```cpp
#pragma omp for schedule(static, chunk_size) reduction(+:suma)
	suma += expresión

#pragma omp for schedule(static, chunk_size) reduction(*:mult)
	mult *= expression
```
## operadores REDUCTION

![[Pasted image 20240226134326.png]]
# Código de directivas 

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