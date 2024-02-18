#CS
## What is Julia?

- **A High-Performance, Dynamic Language:** Julia is designed for numerical and scientific computing. It delivers performance comparable to compiled languages like C while being as dynamic and versatile as languages like Python or R.    
- **Modern Design:** Created in 2012, Julia benefits from the lessons learned from older programming languages. It incorporates a number of advanced features to be both user-friendly and powerful.

## Key Features:

- **Multiple Dispatch:** Unlike traditional object-oriented languages, Julia uses multiple dispatch. This allows functions to be defined based on various combinations of argument types, leading to cleaner, more flexible code.
- **Dynamic Typing:** You don't need to explicitly specify the types of variables, making development feel more like scripting. However, Julia also supports optional type annotations to aid optimization.
- **Just-in-Time (JIT) Compilation:** Julia blends interpretation and compilation. Code is executed quickly while also being continuously optimized by the JIT compiler for greater efficiency.
- **Built-in Package Manager:** Simplifies the process of installing and updating libraries (packages) needed for your work.
- **Great for Parallelism:** Julia has excellent built-in features for parallel computing, making it a strong choice for computationally demanding tasks.

## Why Use Julia?

- **Solving the Two-Language Problem:** Traditionally, scientific programmers would prototype in a dynamic language (Python, R, MATLAB) and then rewrite the performance-critical parts in a lower-level language (C, Fortran) for speed. Julia bridges this gap, letting you develop and execute efficiently within one language.
- **Speed:** Benchmarks demonstrate Julia's remarkable performance, significantly outpacing interpreted languages and often approaching compiled ones.
- **Productivity & Ease of Use:** Julia's syntax is inspired by languages scientists and mathematicians are familiar with. It also provides tools for effortless interactive programming and collaboration.
- **Growing Ecosystem:** Julia has a flourishing community and wide array of available libraries and tools, particularly in domains like:
    - Data Science and Machine Learning
    - Mathematical Optimization
    - Differential Equations
    - Time-Series Analysis
    - Visualization

## Learn More:

- **The Julia Programming Language (Official Site):** [https://julialang.org/](https://julialang.org/)
- **Wikipedia:** [https://en.wikipedia.org/wiki/Julia_(programming_language](https://en.wikipedia.org/wiki/Julia_(programming_language))

# Cosas de la clase 

 Medir tiempo en julia:  
```julia
    @time azar()
```
# Ejemplo de uso de los threads en Julia 

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

**Correlo en la shell de Julia:**
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