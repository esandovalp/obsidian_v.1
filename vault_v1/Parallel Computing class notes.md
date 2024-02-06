#8semester
## 30 de Enero
### Speed Ups
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
	- Paralelismo en la compu:
		```dataviewjs
		const processors = ['S1','S2','S3','S4','S5'];
		const tasks = ['A','B','C','D','E','F','G','H,'I'];

		const datasets = processors.map((processor, index) => ({
			label: processor,
			data: tasks.map((task, taskIndex) => taskIndex + index),
		}));
		const chartData = {
		type: 'bar',
		data: {
			labels: tasks,
			datasets: datasets
			},
			options: {
				scales: {
					x: {
						stacked: true,
					},
					y: {
						stacked: true
					}
				}
			}
		}
		window.renderChart(chartData, document.getElementById('char-container'));
		```

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