# USD/MXN y fundamentos cuantitativos con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance) y practica los fundamentos de probabilidad que hay detrás.

## Qué hace

* Descarga precios históricos de USD/MXN con `yfinance`.
* Calcula rendimientos diarios, volatilidad anualizada y una versión simple del ratio de Sharpe.
* Repite el análisis en varias ventanas de tiempo con una función y un ciclo.
* Grafica el precio, los rendimientos y su distribución con `matplotlib`.
* Mide la volatilidad móvil y prueba un dimensionamiento de posición por volatilidad, evitando el sesgo de look ahead.
* Compara la distribución real de los rendimientos contra una normal: días extremos, curtosis y percentiles en ambas colas.
* Prueba una estrategia de cruce de promedios móviles con costos de operación, caída máxima y validación por mitades del periodo.
* Resuelve problemas de esperanza y varianza y los comprueba con simulaciones Monte Carlo.

## Contenido

* `clase1_usdmxn_volatilidad.ipynb`: rendimientos, volatilidad y anualización.
* `clase2_funciones_ciclos.ipynb`: función `analizar()` con tasa libre de riesgo y comparación entre ventanas.
* `clase3_graficas_distribucion.ipynb`: gráficas, volatilidad móvil, probabilidad empírica, autocorrelación y dimensionamiento por volatilidad.
* `clase4_normal_colas_gordas.ipynb`: puntajes z, curtosis, percentiles reales contra normales y asimetría de las colas.
* `clase5_cruce_promedios.ipynb`: cruce de promedios móviles 10/50, 20/100 y 50/200, costos de operación, sensibilidad al costo y validación por mitades.
* `clase6_esperanza_varianza.ipynb`: fundamentos de probabilidad (no usa datos de USD/MXN). Esperanza, varianza, simulación Monte Carlo de un dado y de una apuesta con ventaja pequeña.

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
* Las simulaciones de la clase 6 son ejercicios didácticos con semilla fija y no representan datos de mercado.

## Próximos pasos

Probabilidad condicional y Bayes, distribuciones y teorema central del límite, álgebra lineal, regresión, EWMA y GARCH, y después estrategias con validación fuera de muestra.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
