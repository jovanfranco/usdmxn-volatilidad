# Bloque 4: Volatilidad y validación (clases 17 a 19)

[Volver al README](../README.md)

## Qué hace

* Modela la volatilidad que cambia con el tiempo: EWMA, GARCH(1,1) con distribución t de Student y GJR sobre los rendimientos de USD/MXN. Estima con el 70% más antiguo de los datos, evalúa en el 30% más reciente con la pérdida QLIKE, revisa los residuos estandarizados con la prueba de Ljung Box, mide si la asimetría mejora el pronóstico fuera de muestra y pronostica la volatilidad a 30 días.
* Mide el efecto de probar muchas estrategias: probabilidad de falsos positivos, cuánto Sharpe sale de puro ruido al probar 1, 20 y 100 estrategias, correcciones de Bonferroni, Holm y Benjamini Hochberg, y una familia de 30 cruces de promedios móviles de USD/MXN con costos, elegida con el 70% más antiguo de los datos y evaluada en el 30% más reciente.
* Evalúa con validación hacia adelante (walk forward) la misma familia de 30 cruces: elige la mejor en cada ventana de entrenamiento, la prueba en los días siguientes y arma un historial continuo fuera de muestra, comparado contra el promedio de las 30 y contra comprar y mantener, con dos tamaños de ventana.

## Contenido

* `clase17_ewma_garch.ipynb`: modelos de volatilidad con `arch`. Ejemplos a mano (EWMA con lambda de 0.94: varianza de 0.295 y volatilidad de 0.543% después de un movimiento de 1.0% con volatilidad previa de 0.5%, y vida media de 11.2 días; GARCH con omega de 0.0072, alfa de 0.08 y beta de 0.90: volatilidad de largo plazo de 0.6%, de 0.715% después de un movimiento de 1.5%, pronósticos de 0.715%, 0.697%, 0.666% y 0.617% a 1, 10, 30 y 100 días, y vida media del choque de 34.3 días; GJR con volatilidad de 0.666% después de un movimiento de más 1.5% y de 0.761% después de uno de menos 1.5%). Con datos reales de USD/MXN de octubre de 2020 a octubre de 2026: EWMA, ventana de 30 días y GARCH(1,1) con t de Student comparados fuera de muestra con QLIKE, prueba de Ljung Box sobre los residuos estandarizados, modelo GJR, comparación fuera de muestra de GARCH contra GJR y pronóstico a 30 días.
* `clase18_pruebas_multiples.ipynb`: pruebas múltiples con `statsmodels` y `scipy`. Ejemplos a mano (probabilidad de al menos un falso positivo con 20 pruebas de 64.2% y umbral de Bonferroni de 0.0025; con p valores de 0.003, 0.011, 0.020, 0.035 y 0.300, Bonferroni rechaza 1, Holm rechaza 2 y Benjamini Hochberg rechaza 4). Simulación de puro ruido con 1500 días y 300 repeticiones: mejor Sharpe esperado de menos 0.02, 0.78 y 1.03 con 1, 20 y 100 estrategias, y error estándar del Sharpe de 0.41 con 6 años. Con datos reales de USD/MXN: 30 cruces de promedios móviles (rápido de 5, 10, 20, 30 y 40 días, lento de 50, 75, 100, 150, 200 y 250 días) con costo de 1 punto base por cambio de posición, p valores con el 70% más antiguo de los datos con y sin corrección, y evaluación de la ganadora y de toda la familia en el 30% más reciente.
* `clase19_walk_forward.ipynb`: validación hacia adelante con la misma familia de 30 cruces. Ejemplos a mano (número de pliegues: 5 con 1500 días, entrenamiento de 750 y prueba de 150, y 12 con 1260 días, entrenamiento de 504 y prueba de 63; con dos estrategias A y B y tres pliegues, las elegidas tienen Sharpe dentro promedio de 0.733 y rendimiento fuera de menos 0.067%, contra menos 0.017% de elegir al azar). Con datos reales de USD/MXN (1320 días, del 23 de septiembre de 2021 al 21 de octubre de 2026): walk forward con entrenamiento de 504 días y prueba de 63 días, y con entrenamiento de 756 días y prueba de 126 días, comparando la estrategia elegida contra el promedio de las 30 y contra comprar y mantener, con Sharpe dentro y fuera de muestra, eficiencia hacia adelante y pliegues positivos.

## Qué aprendí

* La volatilidad no es constante: viene en rachas y regresa a un nivel normal. EWMA le da más peso a los datos recientes con una memoria que se apaga poco a poco (con lambda de 0.94 la vida media es de 11.2 días), y GARCH añade un nivel de largo plazo al que la volatilidad regresa, omega entre uno menos alfa menos beta.
* En un GARCH(1,1), alfa mide cuánto reacciona la volatilidad al movimiento de ayer, beta cuánto recuerda de la volatilidad de ayer, y alfa más beta es la persistencia. La vida media de un choque es ln 0.5 entre ln de la persistencia. Con persistencia cercana a 1 los choques duran mucho, y si pasa de 1 no existe un nivel de largo plazo.
* El pronóstico de GARCH a varios días regresa de forma mecánica al nivel de largo plazo: si la volatilidad de hoy está por debajo, el pronóstico sube. Es un regreso a la media del modelo y no la predicción de un susto, y no dice nada sobre la dirección del precio.
* En USD/MXN (octubre de 2020 a octubre de 2026), el GARCH(1,1) con t de Student estimado en el 70% más antiguo dio una persistencia de 0.888, una vida media de choques de unos 5.8 días y una volatilidad de largo plazo de 0.741% diaria (alrededor de 11.8% anualizada).
* Fuera de muestra, GARCH tuvo el mejor QLIKE (menos 0.176) frente a EWMA (menos 0.159) y a la ventana de 30 días (menos 0.069). La ventaja sobre la ventana móvil es clara, pero la diferencia con EWMA es pequeña y no se le hizo una prueba de significancia.
* Un modelo de volatilidad se evalúa con una pérdida diseñada para varianza (QLIKE), con parámetros estimados solo en el pasado, y se revisan sus residuos: la prueba de Ljung Box sobre los rendimientos al cuadrado dio p cercano a 0 (hay rachas) y sobre los residuos estandarizados al cuadrado dio p de 0.55 (el modelo las captura).
* Con los datos completos, el modelo GJR da un coeficiente de 0.270 a los días en que el dólar sube y de apenas 0.024 a los días en que baja, así que casi toda la reacción de la volatilidad viene de las alzas del dólar (peso débil). El signo de gamma depende de cómo está cotizado el activo: en USD/MXN, el rendimiento positivo es dólar arriba.
* Que la asimetría sea significativa dentro de la muestra no garantiza que mejore el pronóstico: fuera de muestra, GJR tuvo un QLIKE de aproximadamente menos 0.17 contra menos 0.176 de GARCH, así que no mejoró. Una característica descriptiva del pasado puede no servir para pronosticar.
* Con m pruebas independientes sin ventaja real y corte de 5%, la probabilidad de al menos un falso positivo es 1 menos 0.95 elevado a m: 64.2% con 20 pruebas y 92.3% con 50.
* El mejor Sharpe de puro ruido crece con el número de estrategias probadas: aproximadamente 0.78 con 20 y 1.03 con 100, con cerca de 6 años de datos. El error estándar de un Sharpe anual es 1 entre la raíz de los años (0.41 con 6 años). Un Sharpe sin el conteo de estrategias que se probaron para encontrarlo no se puede interpretar.
* Bonferroni (corte de alfa entre m) y Holm controlan la probabilidad de cualquier falso positivo, y Holm nunca es peor que Bonferroni. Benjamini Hochberg controla la proporción esperada de descubrimientos falsos entre los declarados y encuentra más. Con los mismos cinco p valores rechazan 1, 2 y 4.
* Las correcciones suponen pruebas más o menos independientes. Los cruces de promedios móviles comparten mucha información, así que Bonferroni resulta más estricta de lo necesario y el número efectivo de pruebas es menor que 30.
* En USD/MXN, ninguna de las 30 estrategias pasó el 5% ni siquiera sin corregir (la mejor tuvo p de 0.141 y Sharpe de 0.77 dentro de muestra), así que no hubo nada que corregir. Un Sharpe de 0.77 es del mismo orden que el que daría el azar al elegir la mejor entre 30 estrategias sin ventaja (aproximadamente 1 con unos 3.7 años de datos, estimación aproximada porque las 30 no son independientes).
* La ganadora dentro de muestra (cruce 40/75) tuvo Sharpe de 0.77 dentro y 0.51 fuera. No se desplomó, pero con un periodo de prueba de alrededor de 1.6 años el error estándar del Sharpe es de unos 0.8, así que 0.51 no se distingue de cero.
* 25 de las 30 estrategias tuvieron Sharpe positivo fuera de muestra (promedio de 0.29, contra 0.08 dentro). Pero las 30 son muy parecidas entre sí, no son 30 pruebas independientes, y un solo tramo favorable del mercado puede hacer que muchas salgan positivas a la vez, así que no es evidencia de una ventaja.
* Una señal pasa por un ciclo de vida: hipótesis con razón económica, prueba dentro de muestra contando todas las variantes, corrección por pruebas múltiples, prueba fuera de muestra, paper trading, tamaño chico y monitoreo del deterioro. Llevar el registro de todo lo que se prueba es parte del resultado.
* Una sola partición de entrenamiento y prueba es una sola muestra del futuro. La validación hacia adelante la repite: entrena en una ventana, elige, prueba en los días siguientes, avanza y pega todos los tramos de prueba en un historial continuo fuera de muestra. El número de pliegues es el entero de (días totales menos entrenamiento) entre días de prueba. Con ventana móvil el entrenamiento mantiene su tamaño y con ventana creciente se acumula.
* La comparación que dice si el proceso de selección sirve es contra elegir al azar (el promedio de la familia) y contra una referencia simple como comprar y mantener. En el ejemplo a mano, las elegidas tuvieron Sharpe dentro de 0.73 y rendimiento fuera de menos 0.067%, peor que el menos 0.017% de elegir al azar.
* Con 12 pliegues (entrenamiento de 504 días y prueba de 63, 3.0 años fuera de muestra), la estrategia elegida tuvo Sharpe de menos 0.06 fuera de muestra contra 1.15 dentro, una eficiencia hacia adelante de menos 0.05, mientras que el promedio de las 30 tuvo 0.19 y comprar y mantener 0.17.
* Con 4 pliegues (entrenamiento de 756 días y prueba de 126, 2.0 años fuera de muestra), la elegida tuvo 0.51 contra 0.67 del promedio de las 30 y menos 0.31 de comprar y mantener, con eficiencia de 0.46. En las dos configuraciones, elegir con el Sharpe pasado no superó al promedio de la familia, y el resultado cambió de signo y de tamaño con el tamaño de las ventanas, lo que indica que no hay una señal estable.
* Con errores estándar de 0.58 y 0.71, ninguno de los Sharpe fuera de muestra se distingue de cero. Los periodos de prueba de las dos configuraciones son distintos, así que no se comparan uno a uno. Con una ventana de entrenamiento corta, la selección es ruidosa, y el error estándar del Sharpe con 2 años es de cerca de 0.7.
* La eficiencia hacia adelante (Sharpe fuera entre Sharpe dentro de lo elegido) sirve como regla de dedo: cerca de 0.5 o más es alentador y cerca de 0 o negativa suele indicar sobreajuste. Si se ajusta la familia o las ventanas después de ver el resultado, el tramo de prueba se vuelve entrenamiento y hacen falta datos nuevos.

## Conclusión de la clase 17

La volatilidad de USD/MXN se agrupa en rachas: la prueba de Ljung Box sobre los rendimientos al cuadrado rechaza la ausencia de estructura (p cercano a 0), y después de ajustar un GARCH(1,1) con t de Student los residuos estandarizados ya no la muestran (p de 0.55). Fuera de muestra, GARCH tuvo el mejor QLIKE (menos 0.176) frente a EWMA (menos 0.159) y a la ventana de 30 días (menos 0.069). La ventaja sobre la ventana móvil es clara, y la diferencia con EWMA es pequeña y no se probó su significancia. La persistencia estimada es de 0.89, con una vida media de choques de unos 6 días. El modelo GJR con la muestra completa sugiere una asimetría marcada, con las alzas del dólar elevando la volatilidad mucho más que las bajas, pero esa asimetría no mejoró el pronóstico fuera de muestra (QLIKE de aproximadamente menos 0.17 contra menos 0.176 de GARCH). El pronóstico de GARCH regresa al nivel de largo plazo por construcción y sirve para dimensionar riesgo, no para decidir la dirección de una operación.

## Conclusión de la clase 18

Probar muchas estrategias y quedarse con la mejor infla los resultados: con 20 pruebas sin ventaja hay 64.2% de probabilidad de que al menos una parezca significativa, y el mejor Sharpe esperado por puro azar es de aproximadamente 0.78 con 20 estrategias y 1.03 con 100. Las correcciones de Bonferroni, Holm y Benjamini Hochberg ajustan los p valores según el número de pruebas, con distinto grado de rigor, y todas suponen pruebas más o menos independientes. Con una familia de 30 cruces de promedios móviles de USD/MXN, ninguna pasó el 5% ni siquiera sin corregir, así que el resultado es que no hay evidencia de ventaja. La ganadora dentro de muestra (Sharpe de 0.77) tuvo 0.51 fuera de muestra, y 25 de las 30 estrategias salieron positivas fuera de muestra, pero las estrategias están muy correlacionadas, el periodo de prueba es corto y la incertidumbre del Sharpe es grande, así que eso no se interpreta como una ventaja. Un resultado se interpreta junto con el número de pruebas que se hicieron para encontrarlo.

## Conclusión de la clase 19

Una sola partición de entrenamiento y prueba es una sola muestra del futuro, y la validación hacia adelante la repite a lo largo del tiempo para construir un historial continuo fuera de muestra. Con la familia de 30 cruces de promedios móviles de USD/MXN, elegir cada trimestre la mejor estrategia según su Sharpe pasado no superó al promedio de la familia en ninguna de las dos configuraciones probadas: con 12 pliegues la elegida pasó de un Sharpe de 1.15 dentro de muestra a menos 0.06 fuera, contra 0.19 del promedio de las 30, y con 4 pliegues tuvo 0.51 contra 0.67. El resultado cambió de signo y de tamaño con el tamaño de las ventanas y los errores estándar (0.58 y 0.71) son mayores que cualquiera de los Sharpe fuera de muestra, así que no hay evidencia de una señal estable y el proceso de selección no mostró aportar valor. Un historial fuera de muestra sirve para desinflar expectativas antes de arriesgar dinero, y su valor depende de no ajustar el método después de ver el resultado.

## Resultados de la clase 17

Rendimientos diarios de USD/MXN en porcentaje, del 7 de octubre de 2020 al 21 de octubre de 2026. Los modelos se estimaron con el 70% más antiguo de los datos y se evaluaron en el 30% más reciente con parámetros fijos.

GARCH(1,1) con t de Student, estimado en el 70% más antiguo:

| Parámetro | Valor |
|---|---|
| Omega | 0.0616 |
| Alfa | 0.1875 |
| Beta | 0.7002 |
| Grados de libertad de la t | 6.29 |
| Persistencia (alfa más beta) | 0.888 |
| Volatilidad diaria de largo plazo | 0.741% |

Evaluación en el 30% más reciente:

| Modelo | QLIKE (menor es mejor) | Correlación con r al cuadrado | Desviación estándar de r entre volatilidad prevista |
|---|---|---|---|
| EWMA (lambda de 0.94) | menos 0.159 | 0.204 | 1.054 |
| Ventana de 30 días | menos 0.069 | 0.149 | 1.121 |
| GARCH(1,1) | menos 0.176 | 0.267 | 0.889 |

GARCH sobreestimó ligeramente el riesgo en el periodo de prueba (0.889) y EWMA lo subestimó ligeramente (1.054). La prueba de Ljung Box con 10 rezagos dio p cercano a 0 para los rendimientos al cuadrado y p de 0.55 para los residuos estandarizados al cuadrado.

GJR con la muestra completa: alfa de 0.2697, gamma de menos 0.2458, beta de 0.7480 y t con 5.82 grados de libertad. El coeficiente es de 0.270 para los días en que el dólar sube y de 0.024 (alfa más gamma) para los días en que baja. En la evaluación fuera de muestra, el QLIKE de GJR fue de aproximadamente menos 0.17 contra menos 0.176 de GARCH, así que la asimetría no mejoró el pronóstico.

Pronóstico de GARCH desde el último día de la muestra: volatilidad diaria de 0.556%, 0.650% y 0.724% a 1, 10 y 30 días (alrededor de 8.8%, 10.3% y 11.5% anualizada), que sube porque la volatilidad de hoy está por debajo de su nivel de largo plazo.

## Resultados de la clase 18

Treinta cruces de promedios móviles de USD/MXN (rápido de 5, 10, 20, 30 y 40 días, lento de 50, 75, 100, 150, 200 y 250 días), con costo supuesto de 1 punto base por cambio de posición, todas sobre el mismo periodo (1320 días, del 23 de septiembre de 2021 al 21 de octubre de 2026). El p valor es de una prueba t de que el rendimiento diario promedio es cero, calculado con el 70% más antiguo de los datos. El Sharpe es anualizado, sin restar tasa libre.

Las cinco mejores por Sharpe dentro de muestra:

| Rápido | Lento | Sharpe dentro | p valor sin corregir | Sharpe fuera |
|---|---|---|---|---|
| 40 | 75 | 0.77 | 0.141 | 0.51 |
| 30 | 75 | 0.71 | 0.174 | 0.22 |
| 40 | 100 | 0.36 | 0.490 | 0.82 |
| 40 | 50 | 0.29 | 0.576 | 0.05 |
| 30 | 100 | 0.29 | 0.578 | 0.96 |

El p valor corregido de las cinco fue de 1.0 con Bonferroni y con Holm, y de 0.995 con Benjamini Hochberg. Cuántas pasan el 5% con el 70% más antiguo de los datos: 0 sin corregir, 0 con Bonferroni, 0 con Holm y 0 con Benjamini Hochberg.

La ganadora dentro de muestra (cruce 40/75) tuvo Sharpe de 0.77 dentro y de 0.51 fuera. El Sharpe promedio de las 30 estrategias fue de 0.08 dentro y de 0.29 fuera de muestra, y 25 de las 30 tuvieron Sharpe fuera de muestra mayor que cero. Las estrategias mejor posicionadas dentro de muestra no fueron las mejores fuera: por ejemplo, el cruce 30/100 tuvo el mayor Sharpe fuera de muestra de las cinco (0.96) con un Sharpe dentro de apenas 0.29.

Referencia de pura simulación (1500 días, 300 repeticiones, volatilidad diaria de 0.6%): mejor Sharpe esperado de menos 0.02, 0.78 y 1.03 con 1, 20 y 100 estrategias sin ventaja.

## Resultados de la clase 19

Validación hacia adelante de la misma familia de 30 cruces de promedios móviles de USD/MXN, con costo supuesto de 1 punto base por cambio de posición, 1320 días del 23 de septiembre de 2021 al 21 de octubre de 2026. En cada pliegue se elige el cruce de mayor Sharpe en el entrenamiento y se aplica a los días de prueba siguientes. El Sharpe es anualizado, sin restar tasa libre, y el error estándar es aproximado (1 entre la raíz de los años fuera de muestra).

| Concepto | Entrenamiento 504 y prueba 63 | Entrenamiento 756 y prueba 126 |
|---|---|---|
| Pliegues | 12 | 4 |
| Días fuera de muestra | 756 (3.0 años) | 504 (2.0 años) |
| Cruces elegidos | 30/75 (7), 40/75 (3), 40/100 (1), 40/250 (1) | 30/75 (3), 40/75 (1) |
| Sharpe fuera de muestra de la elegida | menos 0.06 | 0.51 |
| Error estándar aproximado | 0.58 | 0.71 |
| Estadístico t aproximado | menos 0.11 | 0.73 |
| Sharpe fuera de muestra del promedio de las 30 | 0.19 | 0.67 |
| Sharpe de comprar y mantener en el mismo periodo | 0.17 | menos 0.31 |
| Sharpe dentro de muestra promedio de las elegidas | 1.15 | 1.12 |
| Eficiencia hacia adelante | menos 0.05 | 0.46 |
| Pliegues con rendimiento positivo | 7 de 12 | 3 de 4 |

En ninguna de las dos configuraciones la elegida superó al promedio de las 30. Que el promedio de las 30 sea positivo en los dos casos (0.19 y 0.67) no se interpreta como una ventaja, porque las 30 estrategias están muy correlacionadas y los errores estándar son grandes. Que comprar y mantener haya tenido signo distinto en los dos periodos (0.17 y menos 0.31) muestra que los periodos de prueba no son los mismos. En el segundo, la ventaja de las estrategias de tendencia sobre comprar y mantener podría deberse a un tramo con tendencia sostenida, una hipótesis que no se verificó.

## Limitaciones

* Las clases 1 a 17 no aplican una corrección por pruebas múltiples, solo se ilustra el problema. En la clase 18 se aplicó a una sola familia de 30 estrategias.
* En la clase 17, los ejemplos a mano usan parámetros inventados. Los parámetros reales vienen de una sola partición de 70% y 30%, y el 30% de prueba es un solo periodo con su propio nivel de volatilidad, por lo que GARCH sobreestimó el riesgo ahí porque su nivel de largo plazo viene de una época más agitada. La diferencia de QLIKE entre GARCH y EWMA es pequeña y no se probó su significancia (por ejemplo con una prueba de Diebold Mariano). La comparación entre GARCH y GJR muestra solo un valor aproximado y una sola partición. El GJR de la muestra completa mezcla datos de entrenamiento y de prueba, y su asimetría es sensible a pocos días extremos, de los cuales los diez mayores (de 2.4% a 4.3% en valor absoluto) no parecen errores de datos pero no se contrastaron con otra fuente. El pronóstico regresa al nivel de largo plazo por construcción y no anticipa cambios de régimen. La distribución t y la especificación GARCH(1,1) son supuestos, y no se probaron otras (EGARCH, más rezagos). Los modelos de volatilidad no dicen nada sobre la dirección del precio.
* En la clase 18, los p valores de ejemplo son inventados y las correcciones suponen pruebas más o menos independientes, algo que no se cumple en 30 cruces de promedios que comparten información, así que Bonferroni y Holm son más estrictas de lo necesario y el número efectivo de pruebas es menor que 30. La familia de 30 combinaciones la elegí yo de forma arbitraria, y el número real de variantes probadas a lo largo del proyecto (incluidas las de la clase 5) es mayor, lo que haría la corrección más estricta. La prueba t supone rendimientos independientes, algo que la volatilidad en rachas no cumple del todo. Hay una sola partición de 70% y 30%, el periodo de prueba es corto (unos 1.6 años) y el error estándar del Sharpe ahí es grande (del orden de 0.8), así que el Sharpe fuera de muestra de la ganadora (0.51) no se distingue estadísticamente de cero. Que 25 de 30 estrategias salgan positivas fuera de muestra no es evidencia de ventaja, porque las estrategias están muy correlacionadas y un mismo tramo del mercado las afecta a todas a la vez. El Sharpe es simple, sin tasa libre de riesgo, y el costo de 1 punto base es un supuesto. Los resultados son descriptivos y no una estrategia lista para operar.
* En la clase 19, los ejemplos a mano usan números inventados. Se usa la misma familia de 30 cruces, el mismo mercado y el mismo periodo de datos de la clase 18, que ya se había revisado, así que no es una validación con datos nuevos y la familia no se eligió antes de ver esos datos. Los dos tamaños de ventana (504 con 63 y 756 con 126) los elegí yo y no son los únicos posibles, y los periodos de prueba que cubren son distintos, así que sus resultados no se comparan uno a uno. La segunda configuración tiene solo 4 pliegues y aporta poca información. Con ventanas de entrenamiento de 2 a 3 años el error estándar del Sharpe es grande, así que la selección es ruidosa. La eficiencia hacia adelante y sus valores de referencia son una regla de dedo y no un criterio estadístico. No se aplicó purga ni embargo, que sería necesario si las señales usaran rendimientos a varios días. El Sharpe es simple, sin tasa libre de riesgo, y el costo de 1 punto base es un supuesto. Los resultados son descriptivos y no una estrategia lista para operar.
