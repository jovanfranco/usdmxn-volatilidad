# USD/MXN: rendimiento y volatilidad con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance).

## Qué hace

- Descarga precios históricos de USD/MXN con `yfinance`.
- Calcula rendimientos diarios, volatilidad anualizada y una versión simple del ratio de Sharpe.
- Repite el análisis en varias ventanas de tiempo con una función y un ciclo.

## Contenido

- `clase1_usdmxn_volatilidad.ipynb`: rendimientos, volatilidad y anualización.
- `clase2_funciones_ciclos.ipynb`: función `analizar()` con tasa libre de riesgo y comparación entre ventanas.

## Qué aprendí

- La volatilidad se anualiza con la raíz de 252 porque la varianza crece de forma lineal con el tiempo.
- El mismo mercado puede verse como excelente o desastroso según la ventana elegida (sesgo de selección). Con ventanas cortas el Sharpe es poco confiable.
- Restar la tasa libre de riesgo cambia el ratio por completo.

## Limitaciones

- No es una estrategia, solo comprar y mantener USD/MXN.
- No incluye carry (diferencial de tasas), costos de transacción ni pruebas fuera de muestra.

## Próximos pasos

Backtests de estrategias simples (tendencia, reversión a la media) con validación en varios periodos.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
