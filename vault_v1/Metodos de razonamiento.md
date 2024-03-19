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

- **RETRIEVE**: revisa/recupera en la memoria de casos qué casos son relevantes para el problema.
- **REUSE**: reutiliza la información/casos en el nuevo problema, generalmente debe de ser adaptada y su nombre alterno es adaptación.
* **PROPOSED SOLUTION**: se propone una solución que puede no ser la mejor, lo cual da lugar a más cambios o más adaptaciones.
