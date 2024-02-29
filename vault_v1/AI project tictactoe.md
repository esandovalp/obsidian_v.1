#cs #ai 
# Importancia de la junta local:

Los tableros que fueron objetivo de los movimientos más recientes son probablemente más importantes. La heurística debería evaluarlos de forma diferente que los tableros más alejados de la "acción" del estado de juego actual.

# Potencial del tablero local: La heurística debería favorecer:

- Tableros en los que está cerca de ganar
- Tableros en los que puede impedir que el adversario gane
- Tableros que todavía tienen muchos movimientos jugables (ofreciendo flexibilidad)

# Perspectiva global: 

Podría incluir un componente menor en su heurística que evalúe el progreso general en el tablero global (pero los tableros locales probablemente serán más dominantes en la evaluación).

