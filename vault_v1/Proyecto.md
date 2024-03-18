La evaluación experimental del rendimiento del algoritmo de agrupamiento K-Means, comparando la ejecución en serie con la ejecución en paralelo utilizando OpenMP a través de diferentes números de hilos, revela varias ideas sobre la escalabilidad del algoritmo y las mejoras de eficiencia debidas a la paralelización.

### Observaciones clave:

1. **OpenMP en serie frente a un único subproceso**:
   - En la mayoría de los casos, la versión en serie del algoritmo tiene un rendimiento comparable o ligeramente superior al de la versión OpenMP con un único hilo. Esta similitud en el rendimiento sugiere que la sobrecarga introducida por OpenMP en la ejecución con un único hilo puede degradar ligeramente el rendimiento. En particular, para conjuntos de datos más grandes (por ejemplo, 300.000 y 400.000 puntos), la versión OpenMP con un único hilo empieza a superar a la versión en serie, lo que indica que algunas optimizaciones o diferencias en las rutas de ejecución dentro del marco OpenMP pueden proporcionar beneficios a medida que aumenta la complejidad de la tarea.

2. **Ganancias de eficiencia con multihilo**:
   - Se observan importantes mejoras de rendimiento a medida que aumenta el número de hilos. El aumento de velocidad oscila entre 4,51 veces con 5 subprocesos para 100.000 puntos y 7,27 veces con 11 subprocesos para 600.000 puntos. Estas mejoras ponen de manifiesto las ventajas del procesamiento paralelo, especialmente para tareas de cálculo intensivo como la agrupación de K-Means en grandes conjuntos de datos.

3. **Rendimiento decreciente con más subprocesos**:
   - Aunque el aumento del número de subprocesos suele mejorar el rendimiento, hay un punto a partir del cual el número de subprocesos adicionales disminuye o, en algunos casos, puede incluso reducir el rendimiento debido a los gastos asociados a la gestión y sincronización de los subprocesos. Por ejemplo, la velocidad tiende a estabilizarse o a disminuir ligeramente al pasar de 11 a 22 subprocesos en varios casos. Esta tendencia sugiere un rango óptimo de hilos para maximizar las ganancias de rendimiento sin incurrir en una sobrecarga excesiva.

4. **Escalabilidad con el tamaño de los datos**:
   - La versión paralelizada del algoritmo se escala más eficazmente con el aumento del tamaño de los datos en comparación con la versión en serie. Los conjuntos de datos más grandes muestran un aumento de velocidad más significativo, lo que pone de manifiesto la ventaja del procesamiento paralelo para las aplicaciones de big data. Sin embargo, la tasa de aumento de velocidad no es lineal con el número de hilos o puntos de datos, lo que indica que el ajuste óptimo del rendimiento requiere una cuidadosa consideración del tamaño del conjunto de datos y de los recursos computacionales disponibles.

5. **Número óptimo de hilos**:
   - El número óptimo de hilos para maximizar el rendimiento varía en función del tamaño del conjunto de datos. Para conjuntos de datos pequeños (por ejemplo, 100.000 puntos), unos 22 subprocesos parecen ofrecer el mejor aumento de velocidad. Para conjuntos de datos más grandes, menos hilos (por ejemplo, 11 hilos para 600.000 puntos) pueden proporcionar una mayor eficiencia. Esta variabilidad subraya la importancia de ajustar el número de subprocesos en función de las características específicas de la carga de trabajo y las capacidades del hardware disponible.

### Conclusión:

La evaluación experimental demuestra que la paralelización del algoritmo K-Means con OpenMP mejora significativamente el rendimiento, especialmente para grandes conjuntos de datos. Sin embargo, los beneficios de los hilos adicionales disminuyen a partir de cierto punto, y el número óptimo de hilos depende tanto del tamaño del conjunto de datos como del entorno computacional específico. Estos resultados ponen de relieve la importancia del ajuste del rendimiento y de las consideraciones de escalabilidad en las aplicaciones de computación paralela.

# Tabla 

|                      |                                                       |                                                   |              |
| -------------------- | ----------------------------------------------------- | ------------------------------------------------- | ------------ |
| **Number of points** | **Serial (avg for 10 i)**                             | **OpenMP (avg for 10 i)**                         | **Speed up** |
| 100,000              | Average time for 100000 points: 190.6 milliseconds.   | Average time with 1 threads: 244.9 milliseconds.  | 0.78x        |
| “                    | “                                                     | Average time with 5 threads: 42.3 milliseconds.   | 4.51x        |
| “                    | “                                                     | Average time with 11: 32.8 milliseconds.          | 5.81x        |
| “                    | “                                                     | Average time with 22 threads: 31 milliseconds.    | 6.15x        |
| 200,000              | Average time for 200000 points: 406.5 milliseconds.   | Average time with 1 threads: 443.7 milliseconds.  | 0.92x        |
| “                    | “                                                     | Average time with 5 threads: 171.5 milliseconds.  | 2.37x        |
| “                    | “                                                     | Average time with 11 threads 152.7 milliseconds.  | 2.66x        |
| "                    | “                                                     | Average time with 22 threads: 66.3 milliseconds.  | 6.13x        |
| 300,000              | Average time for 300000 points: 552.7 milliseconds.   | Average time with 1 threads: 445.6 milliseconds.  | 1.24x        |
| "                    | "                                                     | Average time with 5 threads: 126.6 milliseconds.  | 4.37x        |
| "                    | "                                                     | Average time with 11 threads: 101.6 milliseconds. | 5.44x        |
| "                    | “                                                     | Average time with 22 threads: 95.1 milliseconds.  | 5.81x        |
| 400,000              | Average time for 400000 points: 867.5 milliseconds.   | Average time with 1 threads: 683.3 milliseconds.  | 1.27x        |
| “                    | "                                                     | Average time with 5 threads: 132.5 milliseconds.  | 6.55x        |
| “                    | “                                                     | Average time with 11 threads: 128.8 milliseconds. | 6.74x        |
| “                    | “                                                     | Average time with 22 threads: 189.2 milliseconds. | 4.59x        |
| 600,000              | Average time for 600000 points: 1408.1 milliseconds.  | Average time with 1 threads: 1270.1 milliseconds. | 1.11x        |
| "                    | “                                                     | Average time with 5 threads: 249.7 milliseconds.  | 5.64x        |
| "                    | “                                                     | Average time with 11 threads: 193.7 milliseconds. | 7.27x        |
| “                    | "                                                     | Average time with 22 threads: 284.4 milliseconds. | 4.95x        |
| 800,000              | Average time for 800000 points: 1416.9 milliseconds.  | Average time with 1 threads: 1732.1 milliseconds. | 0.82x        |
| “                    | “                                                     | Average time with 5 threads: 291.4 milliseconds.  | 4.86x        |
| "                    | “                                                     | Average time with 11 threads: 453.9 milliseconds. | 3.12x        |
| “                    | "                                                     | Average time with 22 threads: 213.5 milliseconds. | 6.64x        |
| 1,000,000            | Average time for 1000000 points: 1799.8 milliseconds. | Average time with 1 threads: 1985.8 milliseconds. | 0.91x        |
| “                    | “                                                     | Average time with 5 threads: 382 milliseconds.    | 4.71x        |
| “                    | “                                                     | Average time with 11 threads: 375.5 milliseconds. | 4.79x        |
| “                    | “                                                     | Average time with 22 threads: 276.9 milliseconds. | 6.50x        |