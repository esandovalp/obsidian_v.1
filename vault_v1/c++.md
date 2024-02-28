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

**Acaba intro empieza C++:** [[OpenMP]]

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

//    std::cout << "Donde vive el apuntado " << &apuntador << "\n";
//    std::cout << "Imprimiendo el apuntador " << apuntador << "\n";
//    std::cout << "Imprimiendo el apuntador con el puente" << *apuntador << "\n";


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

# Matrices en C++ 
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

Terminando de ver [[OpenMP]] empezamos con [[MPI]]