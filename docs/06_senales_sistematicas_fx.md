# Bloque 6: Señales sistemáticas en divisas (clases 23 y 24)

[Volver al README](../README.md)

## Qué hace

* Construye una señal de momentum (tendencia) para seis pares de divisas contra el dólar: si el par subió en las últimas L días la posición es larga, y si bajó es corta, con tamaño ajustado por volatilidad (EWMA con lambda de 0.94, volatilidad objetivo de 10% por par y tope de 3 veces).
* Aplica un rezago de un día: la señal calculada al cierre de t gana el rendimiento de t más 1, para no usar información que todavía no se tenía.
* Prueba cuatro ventanas fijadas antes de ver los resultados (21, 63, 126 y 252 días) y un Combo que promedia las cuatro señales, y las compara con una referencia de siempre largo dólar con la misma escala de volatilidad.
* Corrige por pruebas múltiples con Bonferroni (5 pruebas), revisa la estabilidad por mitades y por año calendario, y mide cómo cambian los resultados con costos de 0, 1, 2, 5 y 10 puntos base por unidad operada. No incluye carry.
* Demuestra cuánto infla un resultado usar información del futuro (fuga de información), comparando el Sharpe con y sin rezago para ventanas de 5, 21, 126 y 252 días y con una prueba de control: se quita a cada par su rendimiento promedio, se remuestrean los días al azar (100 repeticiones) y se corre el mismo pipeline, que debe dar un Sharpe cercano a cero.
* Combina tres señales de signo (momentum de 126 días, reversión de 5 días y reversión de 504 días) en un promedio, con la misma escala de volatilidad (objetivo de 10% por par y tope de 3 veces), y compara el Sharpe combinado observado con el que predice la fórmula según la correlación entre señales.
* Corrige con Bonferroni (4 pruebas: las tres señales y la combinada) y mide costos de 0, 1, 2 y 5 puntos base por unidad operada. No incluye carry.

## Contenido

* `clase23_momentum_fx.ipynb`: momentum con `numpy`, `pandas` y `scipy`. Ejemplo a mano con precios de 100, 102, 104, 103, 101, 100 y 102 y una ventana de 3 días: las señales al cierre de los días 3, 4 y 5 son más 1, menos 1 y menos 1, y ganan los rendimientos del día siguiente (menos 1.942%, más 0.990% y menos 2.000%), con un total de menos 2.952% por los cambios de dirección. Con datos reales de seis pares contra el dólar (USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL), se evalúan 1318 días del 28 de septiembre de 2021 al 21 de octubre de 2026 (5.23 años), y los primeros 252 días se usan solo para calentar la señal.
* `clase24_fuga_y_combinar_senales.ipynb`: fuga de información y combinación de señales con `numpy`, `pandas` y `scipy`. Ejemplo a mano de fuga con rendimientos de 1%, menos 2% y 0.5% y señales de signo: con trampa ganan 1%, 2% y 0.5% (total 3.5%) y sin trampa menos 2% y menos 0.5% (total menos 2.5%). Ejemplo a mano de combinación con la fórmula Sharpe combinado igual a S por la raíz de N entre (1 más (N menos 1) por rho), con S de 0.4: para N de 2, 3 y 5 y rho de 0, 0.3 y 0.9 da 0.566, 0.496 y 0.410; 0.693, 0.548 y 0.414; y 0.894, 0.603 y 0.417. Con datos reales de seis pares contra el dólar (USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL) se usan 1,570 días del 8 de octubre de 2020 al 21 de octubre de 2026 y se evalúan 1,065 días desde el 15 de septiembre de 2022 (4.23 años), porque la señal de 504 días necesita calentamiento y todas se miden en las mismas fechas.

## Qué aprendí

* Momentum es una hipótesis económica con una regla simple. En un mercado que cambia de dirección pierde, y el rezago de un día es lo que evita hacer trampa con la información del mismo día.
* En estos datos, ninguna de las cuatro ventanas ni el Combo mostró evidencia de ventaja. Los Sharpe antes de costos fueron menos 0.33 (21 días), 0.14 (63 días), menos 0.22 (126 días), 0.21 (252 días) y menos 0.08 (Combo), con un error estándar de 0.44. Los p fueron de 0.45 a 0.85 y con Bonferroni todos quedaron en 1.0.
* La referencia de siempre largo dólar con la misma escala tuvo un Sharpe de 0.50 (t de 1.15, p de 0.25), mayor que el de todas las variantes de momentum, aunque tampoco significativo.
* El Combo tuvo una correlación de 0.12 con siempre largo dólar: no era simplemente una apuesta al dólar. Con la señal de 126 días, la fracción de días largo dólar fue de 0.70 en USD/JPY, 0.67 en USD/CAD, 0.52 en USD/EUR, 0.51 en USD/GBP, 0.45 en USD/BRL y 0.35 en USD/MXN.
* Las ventanas lentas tuvieron un Sharpe positivo en la primera mitad del periodo (de 0.29 a 0.60) y negativo en la segunda (de menos 0.01 a menos 0.82). El Combo por año fue de 0.44 en 2021 (solo desde el 28 de septiembre), 0.95 en 2022, 0.11 en 2023, 0.23 en 2024, menos 0.83 en 2025 y menos 1.31 en 2026 (hasta el 21 de octubre). Cada año tiene un error estándar cercano a 1, así que estas diferencias no son concluyentes.
* Los costos pesan mucho. La rotación fue de 71.4 unidades por año en la ventana de 21 días, 48.6 en la de 63, 34.8 en la de 252 y 43.4 en el Combo. Con 1 punto base el Sharpe de la ventana de 63 días baja de 0.14 a 0.05, y con 5 puntos base es menos 0.28. Cada punto base cuesta cerca de 0.1 de Sharpe.
* La ausencia de evidencia no es evidencia de ausencia: con cinco años, seis pares y un error estándar de 0.44, el experimento solo detectaría una ventaja grande.
* La fuga de información ocurre cuando la señal de hoy gana el rendimiento de hoy, algo que no se podía saber al armar la posición. El rezago de un día la evita, y por eso el código usa `shift(1)`.
* La fuga infla más mientras más corta es la ventana, porque el rendimiento del propio día pesa más dentro de la señal. El Sharpe con rezago contra el Sharpe con trampa fue de menos 0.03 contra 10.90 con 5 días, menos 0.24 contra 4.67 con 21 días, menos 0.78 contra 1.90 con 126 días y 0.03 contra 1.63 con 252 días.
* La prueba de control sirve para saber si el pipeline es honesto: con datos sin patrón, el Sharpe debe dar cerca de cero. Con rezago dio un promedio de menos 0.02 (desviación estándar de 0.44) y sin rezago 2.14 (desviación estándar de 0.32). Un Sharpe de 2.14 con puro ruido solo se explica por la fuga. Se le quita a cada par su rendimiento promedio antes de remuestrear, porque si no la señal puede "descubrir" la tendencia propia del par y el control sale sesgado.
* Combinar señales ayuda según su correlación: con tres señales de Sharpe 0.4, el combinado es 0.693 si son independientes, 0.548 con correlación de 0.3 y 0.414 si son casi copias (0.9). Y si cada señal tiene Sharpe 0, el combinado es 0: combinar reduce el riesgo, pero no crea ventaja donde no la hay.
* En los datos reales, ninguna señal fue significativa. El momentum de 126 días tuvo un Sharpe de menos 0.78 (t de menos 1.60, p de 0.109, y 0.435 con Bonferroni), la reversión de 5 días 0.03 (p de 0.946) y la de 504 días 0.50 (p de 0.309), con un error estándar del Sharpe de 0.49. La combinada tuvo menos 0.24 (p de 0.622). La referencia de siempre largo dólar tuvo 0.15.
* Las señales tuvieron correlaciones bajas o negativas (momentum contra reversión larga, menos 0.52; momentum contra reversión corta, menos 0.12; reversión corta contra larga, 0.06), con un promedio de menos 0.195. La combinada bajó la volatilidad (2.5%, contra 5.0% a 5.7% de las individuales), pero el Sharpe promedio de las individuales era de menos 0.084, así que el combinado también fue negativo: la fórmula predijo menos 0.186 y se observó menos 0.240.
* El Sharpe del momentum de 126 días fue de menos 0.78 en esta clase y de menos 0.22 en la 23, con la misma regla de escala. La diferencia viene del periodo (esta clase evalúa desde el 15 de septiembre de 2022 y la 23 desde el 28 de septiembre de 2021) y muestra cuánto cambia un resultado de Sharpe con la ventana de datos.
* Los costos pesan según la rotación anual. La reversión de 5 días rota 142.87 unidades por año y pasa de un Sharpe de 0.03 a menos 0.22 con 1 punto base, menos 0.46 con 2 y menos 1.20 con 5. La reversión de 504 días rota 26.98 y pasa de 0.50 a 0.44, 0.39 y 0.23. La combinada rota 59.67 y pasa de menos 0.24 a menos 0.48, menos 0.72 y menos 1.43.
* Un resultado nulo, reportado con claridad y con un control que demuestra que el pipeline es honesto, vale más que un Sharpe espectacular que viene de una trampa.

## Conclusión de la clase 23

Una señal de momentum de precio con ventanas de 1 a 12 meses, escalada por volatilidad y aplicada a seis pares contra el dólar, no mostró evidencia de ventaja entre octubre de 2021 y octubre de 2026: los Sharpe antes de costos fueron de menos 0.33 a 0.21, con un error estándar de 0.44, y ningún p pasó de 0.45 ni siquiera sin corregir. Con costos supuestos de 1 a 10 puntos base por unidad operada, los resultados empeoraron, y la referencia de siempre largo dólar tuvo un Sharpe mayor (0.50), también sin significancia. El periodo es corto, el análisis no incluye carry y las explicaciones posibles del resultado (un periodo sin tendencias sostenidas, la falta de carry o simplemente que no haya ventaja) no están probadas.
## Conclusión de la clase 24

Una fuga de información, usar sin querer datos que no se conocían al operar, puede inflar un resultado hasta volverlo irreconocible: con una señal de 5 días, el Sharpe pasó de menos 0.03 a 10.90. Se detecta con una prueba de control, que corre el mismo proceso sobre datos remuestreados sin patrón real: un proceso honesto da cerca de cero y el tramposo dio 2.14. Combinar tres señales de precio (momentum de 126 días, reversión de 5 días y reversión de 504 días) en seis pares contra el dólar redujo la volatilidad, porque las señales están poco correlacionadas o correlacionadas en negativo, pero no creó ventaja: ninguna señal fue significativa (el p más bajo fue 0.109 y con Bonferroni 0.435), la combinada tuvo un Sharpe de menos 0.24 y los costos de 1 a 5 puntos base lo empeoraron, sobre todo en las señales de rotación alta. El periodo es de 4.23 años, el análisis no incluye carry y las explicaciones posibles (un periodo sin patrones aprovechables o que simplemente no haya ventaja) no están probadas.

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

## Resultados de la clase 24

Evaluación del 15 de septiembre de 2022 al 21 de octubre de 2026 (1,065 días, 4.23 años), con escala de volatilidad de 10% por par, cartera igual ponderada de los seis pares (USD/EUR y USD/GBP invertidos para que los seis sean unidades de moneda extranjera por dólar) y rezago de un día, antes de costos:

| Estrategia | Rend. anual | Volatilidad | Sharpe | t | p | p con Bonferroni |
|---|---|---|---|---|---|---|
| Momentum 126 días | menos 4.47% | 5.72% | menos 0.780 | menos 1.603 | 0.109 | 0.435 |
| Reversión corta 5 días | 0.19% | 5.74% | 0.033 | 0.068 | 0.946 | 1.0 |
| Reversión larga 504 días | 2.48% | 5.01% | 0.495 | 1.018 | 0.309 | 1.0 |
| Combinada (promedio de las 3) | menos 0.60% | 2.50% | menos 0.240 | menos 0.492 | 0.622 | 1.0 |
| Siempre largo USD | 1.10% | 7.48% | 0.147 | 0.303 | 0.762 | no aplica |

El error estándar aproximado del Sharpe es 0.49. Bonferroni se aplicó con 4 pruebas.

Fuga de información (Sharpe con rezago contra sin rezago, momentum, antes de costos):

| Ventana | Con rezago (honesto) | Sin rezago (trampa) |
|---|---|---|
| 5 días | menos 0.03 | 10.90 |
| 21 días | menos 0.24 | 4.67 |
| 126 días | menos 0.78 | 1.90 |
| 252 días | 0.03 | 1.63 |

Prueba de control con datos sin promedio y remuestreados (100 repeticiones, momentum de 126 días): con rezago, promedio de menos 0.02 y desviación estándar de 0.44; sin rezago, promedio de 2.14 y desviación estándar de 0.32. Para la cartera combinada con rezago, promedio de menos 0.02 y desviación estándar de 0.52.

Correlación entre los rendimientos de las señales: momentum contra reversión corta, menos 0.12; momentum contra reversión larga, menos 0.52; reversión corta contra reversión larga, 0.06. Sharpe promedio de las individuales, menos 0.084; correlación promedio, menos 0.195; Sharpe combinado que predice la fórmula, menos 0.186; observado, menos 0.240.

Rotación anual y Sharpe según el costo por unidad operada (los puntos base son un supuesto, no un costo real):

| Estrategia | Rotación anual | 0 pb | 1 pb | 2 pb | 5 pb |
|---|---|---|---|---|---|
| Momentum 126 días | 44.69 | menos 0.78 | menos 0.86 | menos 0.94 | menos 1.17 |
| Reversión corta 5 días | 142.87 | 0.03 | menos 0.22 | menos 0.46 | menos 1.20 |
| Reversión larga 504 días | 26.98 | 0.50 | 0.44 | 0.39 | 0.23 |
| Combinada (promedio de las 3) | 59.67 | menos 0.24 | menos 0.48 | menos 0.72 | menos 1.43 |

## Limitaciones

* Hay una sola muestra de 5.23 años y solo seis pares, todos contra el dólar y muy ligados a un mismo factor. El error estándar del Sharpe es 0.44, así que el análisis solo detecta ventajas grandes, y que no salga una ventaja no demuestra que no exista.
* Los rendimientos son solo de precio y no incluyen el carry (diferencial de tasas), que en divisas es una parte grande del rendimiento de una posición larga o corta.
* Las reglas las elegí yo y no se optimizaron: signo del rendimiento acumulado, ventanas de 21, 63, 126 y 252 días, volatilidad EWMA con lambda de 0.94, volatilidad objetivo de 10% por par, tope de 3 veces y promedio simple entre pares. Otras reglas (señales continuas, otras ventanas, otros pesos) podrían dar otros resultados.
* Los costos de 0 a 10 puntos base por unidad operada son un supuesto mío y no los costos de la mesa. Cambian por par (MXN y BRL cuestan más) y no se modelaron el spread por par, el deslizamiento ni el financiamiento.
* La volatilidad de las carteras (4.2% a 6.0%) quedó por debajo de la objetivo de 10% por par por el efecto de la diversificación, y el Sharpe no cambia con la escala.
* Bonferroni con 5 pruebas es conservador porque las señales están correlacionadas, y el Combo es una sexta construcción que no se corrigió aparte. Los Sharpe por año tienen errores estándar cercanos a 1, y 2021 (desde el 28 de septiembre) y 2026 (hasta el 21 de octubre) son años parciales.
* La referencia de siempre largo dólar no es una estrategia propuesta y su resultado depende de que el dólar haya subido o bajado en el periodo. No es directamente comparable con los Sharpe de la clase 21, que usan otra ventana y otros pesos.
* En la clase 24 hay una sola muestra de 4.23 años y seis pares muy ligados a un mismo factor. El error estándar del Sharpe es 0.49, así que cualquier Sharpe entre menos 0.78 y 0.50 es compatible con cero, y que no salga ventaja no demuestra que no exista.
* El periodo de evaluación de la clase 24 (desde el 15 de septiembre de 2022) es más corto que el de la clase 23 (desde el 28 de septiembre de 2021) porque la señal de 504 días necesita calentamiento, así que los Sharpe de ambas clases no son directamente comparables.
* La combinada es un promedio simple de tres señales elegidas por mí, sin optimizar. Con un Sharpe promedio negativo entre las individuales, combinar no podía producir un resultado positivo, y la fórmula de Sharpe combinado es una aproximación que supone correlaciones y Sharpe parecidos entre señales.
* Bonferroni con 4 pruebas es conservador porque las señales están correlacionadas. La prueba de control solo valida que el pipeline no hace trampa con el rezago, no que las señales tengan ventaja.
* Los costos de 1, 2 y 5 puntos base por unidad operada son un supuesto mío y no los costos de la mesa. No se modelaron el spread por par ni el deslizamiento. Las posiciones son solo de precio, sin carry.
