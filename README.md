# USD/MXN: rendimiento, volatilidad y dimensionamiento de riesgo con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance).

## Qué hace

- Descarga precios históricos de USD/MXN con `yfinance`.
- Calcula rendimientos diarios, volatilidad anualizada y una versión simple del ratio de Sharpe.
- Repite el análisis en varias ventanas de tiempo con una función y un ciclo.
- Grafica el precio, los rendimientos y su distribución con `matplotlib`.
- Mide la volatilidad móvil y prueba un dimensionamiento de posición por volatilidad, evitando el sesgo de look ahead.

## Contenido

- `clase1_usdmxn_volatilidad.ipynb`: rendimientos, volatilidad y anualización.
- `clase2_funciones_ciclos.ipynb`: función `analizar()` con tasa libre de riesgo y comparación entre ventanas.
- `clase3_graficas_distribucion.ipynb`: gráficas, volatilidad móvil, probabilidad empírica, autocorrelación y dimensionamiento por volatilidad.

## Qué aprendí

- La volatilidad se anualiza con la raíz de 252 porque la varianza crece de forma lineal con el tiempo.
- El mismo mercado puede verse como excelente o desastroso según la
