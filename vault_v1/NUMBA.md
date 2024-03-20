# Ordenamiento de burbuja normal
```python
import time
import numpy as np
def ordenamiento_burbuja(arreglo):
	for pos_final in range (len(arreglo), 1, -1):
		for i in range(pos_final -1):
			if arreglo[i] > arreglo[i + 1]:
				tmp = arreglo[i]
				arreglo[i] = arreglo[i+1]
				arreglo[i + 1] = tmp	
				
# Testing the bubble sort function with timing	
arreglo = np.random.rand(10)
ordenamiento_burbuja(arreglo)	
arreglo

%timeit -r 10 -n 30 arreglo1 = np.random.rand(1000); ordenamiento_burbuja(arreglo1)
```

## Output
```bash
269 ms ± 22.3 ms per loop (mean ± std. dev. of 10 runs, 30 loops each)
```

# Ordenamiento de burbuja jit
```python
import time
import numpy as np
from numba import jit

@jit
def ordenamiento_burbuja(arreglo):
	for pos_final in range (len(arreglo), 1, -1):
		for i in range(pos_final -1):
			if arreglo[i] > arreglo[i + 1]:
			tmp = arreglo[i]
			arreglo[i] = arreglo[i+1]
			arreglo[i + 1] = tmp

# Testing the bubble sort function with timing
arreglo = np.random.rand(10)
ordenamiento_burbuja(arreglo)
arreglo

#esto en realidad son 300 ejecuciones
%timeit -r 10 -n 30 arreglo1 = np.random.rand(1000); ordenamiento_burbuja(arreglo1)
%timeit -r 10 -n 30 arreglo2 = np.random.rand(1000); arreglo2.sort()
```
## Output

```bash
1.14 ms ± 73 µs per loop (mean ± std. dev. of 10 runs, 30 loops each) 62.3 µs ± 8.87 µs per loop (mean ± std. dev. of 10 runs, 30 loops each)
```

# Suma de vectores

Falta poner más porque me perdí 

## Código

```python
from numba import vectorize, float64
import numpy as np

def suma_numpy(a, b):
	return a + b

@vectorize( [float64(float64, float64)] )
def suma_numba(a, b):
	return a + b

def suma_tradicional(a, b):
	c = []
	for i in range(len(a)):
		c.append(a[i] + b[i])
	return c
  
vector1 = np.random.rand(10000)
vector2 = np.random.rand(10000)
lista1 = list(vector1)
lista2 = list(vector2)

%timeit -r 10 -n 3000 suma_numpy(vector1, vector2)
%timeit -r 10 -n 3000 suma_numba(vector1, vector2)
%timeit -r 10 -n 3000 suma_numba(lista1, lista2)
%timeit -r 10 -n 3000 suma_tradicional(lista1, lista2)
```
## Output 

```bash
7.32 µs ± 648 ns per loop (mean ± std. dev. of 10 runs, 3000 loops each) 9.22 µs ± 2.06 µs per loop (mean ± std. dev. of 10 runs, 3000 loops each) 1.18 ms ± 159 µs per loop (mean ± std. dev. of 10 runs, 3000 loops each) 1.86 ms ± 200 µs per loop (mean ± std. dev. of 10 runs, 3000 loops each)
```

