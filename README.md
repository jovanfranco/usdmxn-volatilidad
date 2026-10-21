# USD/MXN: rendimiento, volatilidad y riesgo con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance).

## Qué hace

- Descarga precios históricos de USD/MXN con `yfinance`.
- Calcula rendimientos diarios, volatilidad anualizada y una versión simple del ratio de Sharpe.
- Repite el análisis en varias ventanas de tiempo con una función y un ciclo.
- Grafica el precio, los rendimientos y su distribución con `matplotlib`.
- Mide la volatilidad móvil y prueba un dimensionamiento de posición por volatilidad, evitando el sesgo de look ahead.
- Compara la distribución real de los rendimientos contra una normal: días extremos, curtosis y percentiles en ambas colas.

## Contenido

- `clase1_usdmxn_volatilidad.ipynb`: rendimientos, volatilidad y anualización.
- `clase2_funciones_ciclos.ipynb`: función `analizar()` con tasa libre de riesgo y comparación entre ventanas.
- `clase3_graficas_distribucion.ipynb`: gráficas, volatilidad móvil, probabilidad empírica, autocorrelación y dimensionamiento por volatilidad.
- `clase4_normal_colas_gordas.ipynb`: z-scores, curtosis, percentiles reales contra normales y asimetría de las colas.

## Qué aprendí

- La volatilidad se anualiza con la raíz de 252 porque la varianza crece de forma lineal con el tiempo.
- El mismo mercado puede verse como excelente o desastroso según la ventana elegida (sesgo de selección). Con ventanas cortas el Sharpe es poco confiable.
- Restar la tasa libre de riesgo cambia el ratio por completo.
- La volatilidad viene en rachas (agrupamiento): la autocorrelación de los rendimientos es casi cero, pero la de los rendimientos absolutos es claramente positiva. Se puede predecir el tamaño de los movimientos mejor que su dirección.
- Escalar la posición por volatilidad vuelve más pareja la volatilidad de la estrategia, pero no crea rendimiento.
- Para evitar look ahead, la posición de hoy debe usar solo información de ayer (`shift(1)`).
- Los rendimientos tienen colas gordas: 16 días a más de 3 desviaciones contra 3.4 esperados si fuera normal (curtosis en exceso de 3.81).
- La normal se ve razonable en el centro y en el percentil 1% a la baja, pero subestima la cola de subida (el dólar se dispara más de lo que la normal permite). Los días extremos se agrupan en rachas de tormenta.
- Un VaR normal tiene más error para una posición que para la otra, según hacia dónde mire su cola.

## Limitaciones

- No es una estrategia, solo comprar y mantener USD/MXN.
- No incluye carry (diferencial de tasas), costos de transacción ni pruebas fuera de muestra.
- El Sharpe medido en pocos años tiene un margen de error amplio, así que diferencias pequeñas entre versiones no son concluyentes.
- La volatilidad móvil de 30 días reacciona con retraso y el escalado usa un tope de 2x.
- Con solo 16 días extremos, la asimetría de las colas es una pista y no una prueba.

## Próximos pasos

Estrategias simples (tendencia con promedios móviles, reversión a la media) con validación en varios periodos.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
