# USD/MXN y fundamentos cuantitativos con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance) y practica los fundamentos de probabilidad, estadística y álgebra lineal que hay detrás.

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
* Calcula rendimientos y varianza de portafolios con producto punto, matrices y la matriz de covarianza, y mide la diversificación de un portafolio de tres pares de divisas.
* Aplica valores propios y análisis de componentes principales (PCA) a la correlación de tres pares de divisas para encontrar los factores que mueven a los tres a la vez, comprueba la lectura de cada componente contra combinaciones sencillas de los pares y revisa los días más extremos del segundo componente.

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
* `clase11_algebra_lineal.ipynb`: álgebra lineal con `numpy`. Rendimiento de un portafolio como producto punto y matriz por vector, varianza de un portafolio de dos activos con la matriz de covarianza, y un ejemplo con datos reales de tres pares de divisas (USD/MXN, USD/JPY y USD/EUR) que compara la volatilidad del portafolio con el promedio simple de las volatilidades.
* `clase12_valores_propios_pca.ipynb`: valores y vectores propios con ejemplos a mano comprobados en `numpy`, y PCA sobre la matriz de correlación de los rendimientos diarios de USD/MXN, USD/JPY y USD/EUR (octubre de 2020 a octubre de 2026). El primer componente explica 56.9% de la varianza y es un factor dólar (los tres pares con el mismo signo). El segundo explica 28.8% y contrasta al peso (peso 0.78) contra el yen (peso menos 0.63), con el euro casi sin participar. El tercero explica 14.3% y contrasta al euro contra el yen y el peso. También descompone el aporte del peso y del yen en los ocho días más extremos del segundo componente.

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
* El rendimiento de un portafolio es el producto punto de los pesos con los rendimientos, y una matriz por el vector de pesos da la serie completa de rendimientos diarios de una sola vez.
* La varianza de un portafolio depende de las varianzas y de las correlaciones: con correlación 1 no hay diversificación, con correlación 0 la volatilidad de dos activos iguales baja a la raíz de 2 veces menos, y con correlación menos 1 el riesgo puede cancelarse. En forma de matriz es `w @ cov @ w`.
* Con correlaciones menores que 1, la volatilidad de un portafolio resulta menor que el promedio simple de las volatilidades de sus activos.
* Un vector propio es una dirección que una matriz solo estira, y el valor propio es el factor de estiramiento. En una matriz de covarianza o correlación, los vectores propios son combinaciones independientes de activos y los valores propios indican cuánta varianza tiene cada una. Los valores propios de una matriz de correlación suman el número de activos.
* En tres pares de divisas contra el dólar, más de la mitad del movimiento conjunto es un solo factor (56.9%) y dos componentes explican casi 86%, así que agregar más pares contra el dólar diversifica menos de lo que parece.
* PCA da números y no nombres: el nombre de cada componente sale de leer el patrón de sus pesos (mismo signo, signos opuestos, pesos cerca de cero). Comprobar esa lectura contra combinaciones sencillas de los pares confirma que se leyó bien, pero no prueba la explicación económica.
* Un componente que parece un contraste entre dos monedas puede estar dominado por una sola. El segundo componente le da el mayor peso al peso mexicano, así que movimientos grandes del peso por su cuenta también lo activan. Descomponer el aporte de cada par en los días extremos permite distinguir un contraste real de un movimiento de una sola moneda.

## Resultados de la clase 5

Periodo del 7 de octubre de 2020 al 21 de octubre de 2026, con costo supuesto de alrededor de 1 punto base por operación.

| Estrategia | Rendimiento anual | Sharpe | Caída máxima | Cambios de posición |
|---|---|---|---|---|
| Comprar y mantener | menos 1.8% | menos 0.17 | menos 24.9% | 0 |
| Cruce 10/50 | menos 4.4% | menos 0.41 | menos 36.0% | 60 |
| Cruce 20/100 | 0.3% | 0.03 | menos 24.4% | 22 |
| Cruce 50/200 | 0.3% | 0.03 | menos 22.6% | 8 |

Ninguna combinación muestra una ventaja demostrable. El cruce 10/50 pierde en las dos mitades del periodo. Los Sharpe de 20/100 y 50/200 son casi cero y cambian de signo entre mitades, así que son ruido.

## Resultados de la clase 12

PCA sobre la matriz de correlación de los rendimientos diarios de USD/MXN, USD/JPY y USD/EUR (cotizado como dólares por euro invertido, para que los tres sean unidades de moneda extranjera por dólar), del 7 de octubre de 2020 al 21 de octubre de 2026. El signo global de cada componente es arbitrario.

| Componente | Varianza explicada | Peso EUR | Peso JPY | Peso MXN | Lectura |
|---|---|---|---|---|---|
| 1 | 56.9% | menos 0.67 | menos 0.56 | menos 0.49 | Factor dólar: los tres pares se mueven juntos |
| 2 | 28.8% | menos 0.05 | menos 0.63 | 0.78 | Contraste entre peso y yen, dominado por el peso, con el euro casi fuera |
| 3 | 14.3% | menos 0.74 | 0.54 | 0.39 | Euro contra yen y peso |

El puntaje diario del primer componente tiene una correlación de menos 1.0 (el signo es arbitrario) con el promedio de los tres pares estandarizados, y el del segundo tiene 0.99 con la diferencia entre el peso y el yen. Esto confirma la lectura de los pesos, pero no prueba la explicación económica de los componentes.

Los ocho días con mayor valor absoluto del segundo componente son el 5 de agosto de 2024, el 4 de junio de 2024, el 14 de marzo de 2023, el 7 y 11 de abril de 2025, el 4 de febrero de 2025, el 9 de agosto de 2024 y el 10 de abril de 2025. Las fechas se asociaron con episodios de estrés de mercado a partir de memoria y no se contrastaron con una fuente de noticias. En 7 de los 8 días el peso y el yen se movieron en direcciones opuestas, y en 5 el yen aportó una parte importante del valor. En los otros 3 (4 de junio de 2024, 7 de abril de 2025 y 4 de febrero de 2025) el valor lo explica sobre todo el peso. El componente es por tanto un contraste entre peso y yen dominado por el peso, y la explicación económica (aversión al riesgo) sigue siendo una hipótesis.

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
* En la clase 11, la matriz de covarianza se estima con datos históricos y supone que las correlaciones son estables. En épocas de estrés las correlaciones suelen subir, así que la diversificación observada en periodos tranquilos puede sobrestimar la real. Los tres pares de divisas son un ejemplo didáctico de pocos activos y no un portafolio optimizado.
* En la clase 12, el PCA usa solo tres activos y un periodo específico, así que los componentes pueden cambiar con otros pares o con otras fechas. La comprobación de correlación casi perfecta es parcialmente circular, porque los componentes se construyen con los mismos tres pares. La lectura del segundo componente como aversión al riesgo es una hipótesis: las fechas extremas coinciden con episodios de estrés, pero no se verificaron con fuentes y en 3 de los 8 días el valor lo explica sobre todo el peso. Las fechas de Yahoo Finance pueden desfasarse respecto a la fecha del evento, así que la asociación con eventos es aproximada. El tercer componente explica poco y puede mezclar factores específicos con ruido. Como se usó la matriz de correlación, los porcentajes hablan de movimiento compartido y no de la varianza de un portafolio concreto.

## Próximos pasos

Cálculo y optimización (derivadas y mínimos cuadrados), regresión, series de tiempo (ARIMA y GARCH), corrección por pruebas múltiples, y después estrategias con validación fuera de muestra.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
