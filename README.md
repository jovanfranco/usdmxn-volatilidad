# USD/MXN y fundamentos cuantitativos con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance) y practica los fundamentos de probabilidad y estadística que hay detrás.

## Qué hace

* Descarga precios históricos de USD/MXN con `yfinance`.
* Calcula rendimientos diarios, volatilidad anualizada y una versión simple del ratio de Sharpe.
* Repite el análisis en varias ventanas de tiempo con una función y un ciclo.
* Grafica el precio, los rendimientos y su distribución con `matplotlib`.
* Mide la volatilidad móvil y prueba un dimensionamiento de posición por volatilidad, evitando el sesgo de look ahead.
* Compara la distribución real de los rendimientos contra una normal: días extremos, curtosis y percentiles en ambas colas.
* Prueba una estrategia de cruce de promedios móviles con costos de operación, caída máxima y validación por mitades del periodo.
* Resuelve problemas de esperanza y varianza y los comprueba con simulaciones Monte Carlo.
* Calcula probabilidades condicionales y aplica el teorema de Bayes con un ejemplo ilustrativo.
* Simula la distribución binomial, la ley de los grandes números y el teorema central del límite.
* Aplica pruebas de hipótesis, p-valores e intervalos de confianza, y calcula el estadístico t de un rendimiento promedio de USD/MXN.
* Resuelve problemas clásicos de probabilidad de entrevista (martingalas, ruina del apostador, análisis del primer paso, Monty Hall, cumpleaños y coleccionista) y los comprueba con simulaciones.

## Contenido

* `clase1_usdmxn_volatilidad.ipynb`: rendimientos, volatilidad y anualización.
* `clase2_funciones_ciclos.ipynb`: función `analizar()` con tasa libre de riesgo y comparación entre ventanas.
* `clase3_graficas_distribucion.ipynb`: gráficas, volatilidad móvil, probabilidad empírica, autocorrelación y dimensionamiento por volatilidad.
* `clase4_normal_colas_gordas.ipynb`: puntajes z, curtosis, percentiles reales contra normales y asimetría de las colas.
* `clase5_cruce_promedios.ipynb`: cruce de promedios móviles 10/50, 20/100 y 50/200, costos de operación, sensibilidad al costo y validación por mitades.
* `clase6_esperanza_varianza.ipynb`: fundamentos de probabilidad (no usa datos de USD/MXN). Esperanza, varianza, simulación Monte Carlo de un dado y de una apuesta con ventaja pequeña.
* `clase7_condicional_bayes.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Probabilidad condicional, independencia y teorema de Bayes con un ejemplo ilustrativo de 100 días.
* `clase8_distribuciones_tcl.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Distribución binomial, regla de 68, 95 y 99.7, error estándar, ley de los grandes números y teorema central del límite con simulaciones.
* `clase9_hipotesis_confianza.ipynb`: pruebas de hipótesis y p-valor exacto con la binomial (60 de 100 aciertos da 2.8% y 55 de 100 da 18.4%). Intervalo de confianza del rendimiento diario de USD/MXN de octubre de 2020 a octubre de 2026: de menos 0.042% a 0.026%, que incluye el cero (estadístico t de menos 0.47). También calcula la probabilidad de falsos positivos al probar 20 estrategias (64.2%).
* `clase10_probabilidad_entrevistas.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Simulaciones de ruina del apostador con y sin ventaja, tiempo esperado hasta ver un patrón de caras y cruces, problema del cumpleaños, Monty Hall y coleccionista de caras del dado, comparadas con su solución teórica.

## Qué aprendí

* La volatilidad se anualiza con la raíz de 252 porque la varianza de eventos independientes se suma con el tiempo.
* El mismo mercado puede verse como excelente o desastroso según la ventana elegida (sesgo de selección). Con ventanas cortas el Sharpe es poco confiable.
* Restar la tasa libre de riesgo cambia el ratio por completo.
* La volatilidad viene en rachas: la autocorrelación de los rendimientos es casi cero, pero la de los rendimientos absolutos es claramente positiva. Se puede predecir el tamaño de los movimientos mejor que su dirección.
* Escalar la posición por volatilidad vuelve más pareja la volatilidad de la estrategia, pero no crea rendimiento.
* Para evitar look ahead, la posición de hoy debe usar solo información de ayer (`shift(1)`).
* Los rendimientos tienen colas gordas: 16 días a más de 3 desviaciones contra 3.4 esperados si fuera normal (curtosis en exceso de 3.81).
* La normal se ve razonable en el centro y en el percentil 1% a la baja, pero subestima la cola de subida. Los días extremos se agrupan en rachas de tormenta.
* Un VaR normal tiene más error para una posición que para la otra, según hacia dónde mire su cola.
* Un detalle de código puede mover el resultado más que la estrategia misma: tratar mal los días en que el promedio largo todavía no existe generaba un corto falso al inicio.
* Para comparar estrategias hay que medirlas en el mismo periodo y fijar la fecha final de los datos, para que los resultados se puedan reproducir.
* Con un costo supuesto de alrededor de 1 punto base por operación, el costo pesa poco en un cruce de promedios. Quitar costos no crea una ventaja que la señal no tiene.
* La esperanza de una suma es la suma de las esperanzas, aunque haya dependencia. La varianza de eventos independientes se suma, y la desviación estándar crece con la raíz de n.
* Una apuesta con ventaja positiva (55% de ganar 1 peso, 45% de perder 1) tuvo esperanza de 10 en 100 apuestas, pero 13.3% de los jugadores simulados terminó en pérdida. La esperanza sola no basta: hay que medir también la dispersión.
* La probabilidad condicional cambia el universo que se mira: filtrar por una condición modifica la probabilidad. Dos eventos son independientes solo si saber uno no cambia la probabilidad del otro, y no se deben multiplicar probabilidades sin verificarlo.
* P(A dado B) y P(B dado A) son preguntas distintas con respuestas distintas. Bayes permite voltear la condicional y actualizar una creencia con evidencia nueva.
* Con una ventaja real (por ejemplo 55% de acierto) y pocas operaciones, perder la mayoría sigue siendo bastante probable: 26% con 10 operaciones.
* El error estándar del promedio es la desviación estándar entre la raíz de n. Para reducirlo a la décima parte hacen falta 100 veces más datos, y por eso el Sharpe medido en pocos años tiene un margen de error grande.
* El teorema central del límite hace que los promedios de muchas variables independientes se parezcan a una normal, pero depende de la independencia y converge lento cuando hay colas gordas, así que en mercado funciona solo como aproximación.
* El p-valor es la probabilidad de ver datos tan buenos como los observados si la hipótesis nula fuera cierta. No es la probabilidad de que la estrategia sirva: confundirlas es el mismo error de P(A dado B) contra P(B dado A).
* Un intervalo de confianza muestra entre qué valores probablemente está el efecto verdadero. Si incluye el cero, no se puede descartar que no haya ventaja. Con casi seis años de datos, el rendimiento anual de comprar y mantener USD/MXN queda en un rango de aproximadamente menos 10% a 6%, que incluye el cero.
* Como guía aproximada, el estadístico t de un Sharpe es el Sharpe por la raíz de los años de datos. Con un Sharpe pequeño hacen falta muchos años para tener evidencia.
* Probar muchas estrategias infla los falsos positivos: con 20 pruebas sin ventaja y corte de 5%, la probabilidad de que al menos una salga significativa por azar es de 64%. Lo relevante no es el p-valor de la ganadora, sino cuántas se probaron.
* Un juego justo (martingala) no se le gana con ninguna estrategia de apuestas si el capital es finito. Duplicar tras cada pérdida gana poco casi siempre y pierde mucho casi nunca, con esperanza cero.
* Con una ventaja real, el capital inicial decide el riesgo de ruina: con 55% de acierto y apuestas de una unidad, la ruina eventual es de 13.4% con 10 unidades de capital y de 1.8% con 20. El tamaño de posición pesa tanto como la ventaja.
* El análisis del primer paso convierte problemas de esperanza en ecuaciones sencillas. Esperar HH toma 6 lanzamientos en promedio y esperar HT toma 4, aunque ambos patrones tengan la misma probabilidad de aparecer en dos lanzamientos.
* Las variables indicadoras y la linealidad de la esperanza resuelven problemas como el de los sombreros sin calcular distribuciones completas.

## Resultados de la clase 5

Periodo del 7 de octubre de 2020 al 21 de octubre de 2026, con costo supuesto de alrededor de 1 punto base por operación.

| Estrategia | Rendimiento anual | Sharpe | Caída máxima | Cambios de posición |
|---|---|---|---|---|
| Comprar y mantener | menos 1.8% | menos 0.17 | menos 24.9% | 0 |
| Cruce 10/50 | menos 4.4% | menos 0.41 | menos 36.0% | 60 |
| Cruce 20/100 | 0.3% | 0.03 | menos 24.4% | 22 |
| Cruce 50/200 | 0.3% | 0.03 | menos 22.6% | 8 |

Ninguna combinación muestra una ventaja demostrable. El cruce 10/50 pierde en las dos mitades del periodo. Los Sharpe de 20/100 y 50/200 son casi cero y cambian de signo entre mitades, así que son ruido.

## Limitaciones

* No hay una estrategia con ventaja demostrada. Los resultados son de aprendizaje.
* No incluye carry (diferencial de tasas) ni pruebas fuera de muestra. El costo de operación es un supuesto simple y fijo.
* El Sharpe medido en pocos años tiene un margen de error amplio, así que diferencias pequeñas entre versiones no son concluyentes.
* La volatilidad móvil de 30 días reacciona con retraso y el escalado usa un tope de 2x.
* Con solo 16 días extremos, la asimetría de las colas es una pista y no una prueba.
* Se probaron solo tres combinaciones de ventanas. Buscar la mejor entre muchas llevaría a sobreajuste.
* Las clases 6, 7, 8 y 10 son ejercicios didácticos. Las simulaciones usan semilla fija y el ejemplo de noticias y movimientos de la clase 7 son 100 días hipotéticos, no datos reales de mercado.
* En la clase 9, el intervalo de confianza y el estadístico t suponen rendimientos independientes. Eso no se cumple del todo en USD/MXN por la volatilidad en rachas, así que son una aproximación.
* Todavía no se aplica una corrección formal por pruebas múltiples, solo se ilustra el problema.
* Los modelos de ruina de la clase 10 suponen apuestas independientes de tamaño fijo y probabilidad constante, algo que los mercados no cumplen de forma estricta.

## Próximos pasos

Álgebra lineal y cálculo, regresión, series de tiempo (ARIMA y GARCH), corrección por pruebas múltiples, y después estrategias con validación fuera de muestra.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
