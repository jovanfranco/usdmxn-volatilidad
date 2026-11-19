# Bloque 6: Señales sistemáticas en divisas (clase 23)

[Volver al README](../README.md)

## Qué hace

* Construye una señal de momentum (tendencia) para seis pares de divisas contra el dólar: si el par subió en las últimas L días la posición es larga, y si bajó es corta, con tamaño ajustado por volatilidad (EWMA con lambda de 0.94, volatilidad objetivo de 10% por par y tope de 3 veces).
* Aplica un rezago de un día: la señal calculada al cierre de t gana el rendimiento de t más 1, para no usar información que todavía no se tenía.
* Prueba cuatro ventanas fijadas antes de ver los resultados (21, 63, 126 y 252 días) y un Combo que promedia las cuatro señales, y las compara con una referencia de siempre largo dólar con la misma escala de volatilidad.
* Corrige por pruebas múltiples con Bonferroni (5 pruebas), revisa la estabilidad por mitades y por año calendario, y mide cómo cambian los resultados con costos de 0, 1, 2, 5 y 10 puntos base por unidad operada. No incluye carry.

## Contenido

* `clase23_momentum_fx.ipynb`: momentum con `numpy`, `pandas` y `scipy`. Ejemplo a mano con precios de 100, 102, 104, 103, 101, 100 y 102 y una ventana de 3 días: las señales al cierre de los días 3, 4 y 5 son más 1, menos 1 y menos 1, y ganan los rendimientos del día siguiente (menos 1.942%, más 0.990% y menos 2.000%), con un total de menos 2.952% por los cambios de dirección. Con datos reales de seis pares contra el dólar (USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL), se evalúan 1318 días del 28 de septiembre de 2021 al 21 de octubre de 2026 (5.23 años), y los primeros 252 días se usan solo para calentar la señal.

## Qué aprendí

* Momentum es una hipótesis económica con una regla simple. En un mercado que cambia de dirección pierde, y el rezago de un día es lo que evita hacer trampa con la información del mismo día.
* En estos datos, ninguna de las cuatro ventanas ni el Combo mostró evidencia de ventaja. Los Sharpe antes de costos fueron menos 0.33 (21 días), 0.14 (63 días), menos 0.22 (126 días), 0.21 (252 días) y menos 0.08 (Combo), con un error estándar de 0.44. Los p fueron de 0.45 a 0.85 y con Bonferroni todos quedaron en 1.0.
* La referencia de siempre largo dólar con la misma escala tuvo un Sharpe de 0.50 (t de 1.15, p de 0.25), mayor que el de todas las variantes de momentum, aunque tampoco significativo.
* El Combo tuvo una correlación de 0.12 con siempre largo dólar: no era simplemente una apuesta al dólar. Con la señal de 126 días, la fracción de días largo dólar fue de 0.70 en USD/JPY, 0.67 en USD/CAD, 0.52 en USD/EUR, 0.51 en USD/GBP, 0.45 en USD/BRL y 0.35 en USD/MXN.
* Las ventanas lentas tuvieron un Sharpe positivo en la primera mitad del periodo (de 0.29 a 0.60) y negativo en la segunda (de menos 0.01 a menos 0.82). El Combo por año fue de 0.44 en 2021 (solo desde el 28 de septiembre), 0.95 en 2022, 0.11 en 2023, 0.23 en 2024, menos 0.83 en 2025 y menos 1.31 en 2026 (hasta el 21 de octubre). Cada año tiene un error estándar cercano a 1, así que estas diferencias no son concluyentes.
* Los costos pesan mucho. La rotación fue de 71.4 unidades por año en la ventana de 21 días, 48.6 en la de 63, 34.8 en la de 252 y 43.4 en el Combo. Con 1 punto base el Sharpe de la ventana de 63 días baja de 0.14 a 0.05, y con 5 puntos base es menos 0.28. Cada punto base cuesta cerca de 0.1 de Sharpe.
* La ausencia de evidencia no es evidencia de ausencia: con cinco años, seis pares y un error estándar de 0.44, el experimento solo detectaría una ventaja grande.

## Conclusión de la clase 23

Una señal de momentum de precio con ventanas de 1 a 12 meses, escalada por volatilidad y aplicada a seis pares contra el dólar, no mostró evidencia de ventaja entre octubre de 2021 y octubre de 2026: los Sharpe antes de costos fueron de menos 0.33 a 0.21, con un error estándar de 0.44, y ningún p pasó de 0.45 ni siquiera sin corregir. Con costos supuestos de 1 a 10 puntos base por unidad operada, los resultados empeoraron, y la referencia de siempre largo dólar tuvo un Sharpe mayor (0.50), también sin significancia. El periodo es corto, el análisis no incluye carry y las explicaciones posibles del resultado (un periodo sin tendencias sostenidas, la falta de carry o simplemente que no haya ventaja) no están probadas.

## Resultados de la clase 23

Evaluación del 28 de septiembre de 2021 al 21 de octubre de 2026 (5.23 años), escala de volatilidad de 10% por par y cartera igual ponderada de los seis pares, antes de costos:

| Estrategia | Rend. anual | Volatilidad | Sharpe | t | p | Caída máxima | p con Bonferroni |
|---|---|---|---|---|---|---|---|
| Momentum 21 días | menos 1.98% | 5.99% | menos 0.330 | menos 0.755 | 0.450 | menos 14.95% | 1.0 |
| Momentum 63 días | 0.80% | 5.82% | 0.137 | 0.313 | 0.754 | menos 10.36% | 1.0 |
| Momentum 126 días | menos 1.27% | 5.74% | menos 0.220 | menos 0.504 | 0.614 | menos 21.99% | 1.0 |
| Momentum 252 días | 1.08% | 5.22% | 0.207 | 0.474 | 0.635 | menos 9.18% | 1.0 |
| Combo (promedio de las 4) | menos 0.34% | 4.23% | menos 0.081 | menos 0.184 | 0.854 | menos 10.37% | 1.0 |
| Siempre largo USD | 3.76% | 7.50% | 0.501 | 1.146 | 0.252 | menos 11.75% | no aplica |

El error estándar aproximado del Sharpe es 0.44. Bonferroni se aplicó con 5 pruebas, y es conservador porque las señales están correlacionadas.

Sharpe por mitad del periodo:

| Estrategia | Primera mitad | Segunda mitad |
|---|---|---|
| Momentum 21 días | menos 0.30 | menos 0.36 |
| Momentum 63 días | 0.29 | menos 0.01 |
| Momentum 126 días | 0.38 | menos 0.82 |
| Momentum 252 días | 0.60 | menos 0.16 |
| Combo | 0.31 | menos 0.45 |
| Siempre largo USD | 0.67 | 0.34 |

Sharpe del Combo por año calendario: 2021, 0.44; 2022, 0.95; 2023, 0.11; 2024, 0.23; 2025, menos 0.83; 2026, menos 1.31.

Fracción de días largo dólar con la señal de 126 días: USD/MXN 0.35, USD/JPY 0.70, USD/EUR 0.52, USD/GBP 0.51, USD/CAD 0.67 y USD/BRL 0.45. Correlación del Combo con siempre largo USD: 0.12.

Rotación anual y Sharpe según el costo por unidad operada (los puntos base son un supuesto, no un costo real):

| Estrategia | Rotación anual | 0 pb | 1 pb | 2 pb | 5 pb | 10 pb |
|---|---|---|---|---|---|---|
| Momentum 21 días | 71.40 | menos 0.33 | menos 0.45 | menos 0.57 | menos 0.92 | menos 1.51 |
| Momentum 63 días | 48.64 | 0.14 | 0.05 | menos 0.03 | menos 0.28 | menos 0.70 |
| Momentum 252 días | 34.83 | 0.21 | 0.14 | 0.07 | menos 0.13 | menos 0.46 |
| Combo (promedio de las 4) | 43.37 | menos 0.08 | menos 0.18 | menos 0.29 | menos 0.59 | menos 1.10 |

## Limitaciones

* Hay una sola muestra de 5.23 años y solo seis pares, todos contra el dólar y muy ligados a un mismo factor. El error estándar del Sharpe es 0.44, así que el análisis solo detecta ventajas grandes, y que no salga una ventaja no demuestra que no exista.
* Los rendimientos son solo de precio y no incluyen el carry (diferencial de tasas), que en divisas es una parte grande del rendimiento de una posición larga o corta.
* Las reglas las elegí yo y no se optimizaron: signo del rendimiento acumulado, ventanas de 21, 63, 126 y 252 días, volatilidad EWMA con lambda de 0.94, volatilidad objetivo de 10% por par, tope de 3 veces y promedio simple entre pares. Otras reglas (señales continuas, otras ventanas, otros pesos) podrían dar otros resultados.
* Los costos de 0 a 10 puntos base por unidad operada son un supuesto mío y no los costos de la mesa. Cambian por par (MXN y BRL cuestan más) y no se modelaron el spread por par, el deslizamiento ni el financiamiento.
* La volatilidad de las carteras (4.2% a 6.0%) quedó por debajo de la objetivo de 10% por par por el efecto de la diversificación, y el Sharpe no cambia con la escala.
* Bonferroni con 5 pruebas es conservador porque las señales están correlacionadas, y el Combo es una sexta construcción que no se corrigió aparte. Los Sharpe por año tienen errores estándar cercanos a 1, y 2021 (desde el 28 de septiembre) y 2026 (hasta el 21 de octubre) son años parciales.
* La referencia de siempre largo dólar no es una estrategia propuesta y su resultado depende de que el dólar haya subido o bajado en el periodo. No es directamente comparable con los Sharpe de la clase 21, que usan otra ventana y otros pesos.
