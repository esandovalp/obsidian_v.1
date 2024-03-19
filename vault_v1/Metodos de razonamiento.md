Estrategias, algoritmos, planes, recetas, secuencias de pasos estandarizados.

# Basado en reglas

Son tipos de reglas declarativas, porque la recomendación que dan están basadas en características (Expert Systems Programming ... , Ken Pedersen). 

Conjunto de variables de importancia: estado

1. Comparar el estado actual con las pre condiciones (si estamos haciendo encadenamiento hacia delante) para determinar que reglas son aplicables
2. Elegir una de las reglas aplicables ("resolución de conflictos")
	1. Aleatoriamente
	2. La 1a de las aplicables (sesgada a favor de las reglas que aparecen después)
	3. La ultima de las aplicables
	4. La menos restrictiva de las aplicables (descarta pocas opciones, hay varios caminos en lugar de acotar, reglas más simples)
	5. La más restrictiva (sesgar a reglas más completas)
	6. La menos frecuentemente utilizada (favorecer a las que menos han sido utilizadas)
	7. La más frecuentemente utilizada (puede que sean las mejores)
3. Utilizar/disparar/activar la regla elegida (resultado en un cambio de estado)

# Basado en casos


