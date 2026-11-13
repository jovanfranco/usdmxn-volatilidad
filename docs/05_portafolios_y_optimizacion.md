# Bloque 5: Portafolios y optimización (clases 20 y 21)

[Volver al README](../README.md)

## Qué hace

* Encuentra el portafolio de varianza mínima de dos activos con una fórmula cerrada y muestra la solución de esquina que aparece al prohibir las ventas en corto.
* Resuelve el portafolio de varianza mínima de N activos con álgebra lineal (sin restricciones) y con optimización numérica (solo largo y con tope por activo), y comprueba la esquina con la covarianza de cada activo contra la cartera.
* Compara cinco carteras de seis pares de divisas contra el dólar (pesos iguales, inverso de la volatilidad, mínima varianza libre, mínima varianza solo largo y solo largo con tope de 40%), estimando la covarianza con el 70% más antiguo de los datos y midiendo la volatilidad realizada en el 30% más reciente.
* Construye el portafolio de máximo Sharpe y la frontera eficiente, muestra con ejemplos cómo cambian los pesos por pequeños errores en los rendimientos esperados, y simula con 300 repeticiones cuánto daña el error de estimación a distintas carteras cuando se conocen los parámetros verdaderos.
* Aplica shrinkage (encogimiento) a la covarianza con Ledoit Wolf y a los rendimientos esperados hacia su promedio, y compara cuatro carteras de seis pares de divisas con datos reales, estimando con el 70% más antiguo de los datos y evaluando en el 30% más reciente.

## Contenido

* `clase20_portafolios_esquina.ipynb`: optimización de portafolios con `numpy` y `scipy`. Ejemplos a mano con dos activos de volatilidades de 10% y 20%: el peso óptimo en el activo tranquilo es 0.80, 0.895, 1.0 y 1.333 con correlaciones de 0, 0.3, 0.5 y 0.8, el umbral de correlación es 0.5 (volatilidad baja entre volatilidad alta), y con correlación de 0.8 la solución libre (peso de 1.333, es decir 33% vendido corto en el otro activo) tiene volatilidad de 8.94% contra 10% en la esquina de solo largo. Con tres activos de volatilidades de 10%, 15% y 20%: sin restricción los pesos son 1.069, 0.264 y menos 0.333 (volatilidad de 7.84%), y solo largo son 0.736, 0.264 y 0 (volatilidad de 9.03%), con covarianzas de cada activo con la cartera de 0.00815, 0.00815 y 0.01415 contra una varianza de la cartera de 0.00815. Con datos reales de USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL (1570 días de rendimientos comunes de octubre de 2020 a octubre de 2026): cinco carteras entrenadas con 1099 días y evaluadas en los 471 días siguientes.
* `clase21_markowitz_shrinkage.ipynb`: Markowitz completo con `numpy`, `scipy` y `scikit-learn`. Ejemplos a mano con dos activos de rendimientos esperados de 4% y 8%, volatilidades de 10% y 20% y correlación de 0.3: pesos de máximo Sharpe de 2/3 y 1/3, rendimiento de 5.33%, volatilidad de 10.75% y Sharpe de 0.496, contra 0.40 de cada activo por separado, y una frontera con el peso del primer activo de 0 a 1. Dos activos casi idénticos (volatilidad de 10% y correlación de 0.9) cuyos pesos pasan de 4.8% y 95.2% a 95.2% y 4.8% al cambiar medio punto el rendimiento esperado, contra un error estándar del rendimiento promedio de 5 puntos con 4 años de datos. Simulación de seis activos (300 repeticiones, 1000 días) en dos escenarios, con Sharpe verdadero parecido y muy distinto entre activos. Con datos reales de seis pares contra el dólar (1570 días, del 8 de octubre de 2020 al 21 de octubre de 2026, 1099 de entrenamiento y 471 de prueba): pesos iguales, mínima varianza con covarianza encogida, máximo Sharpe sin encoger y máximo Sharpe con rendimientos encogidos (lambda de 0.25). El notebook incluye además una parte de carry (diferencial de tasas) y de elección de lambda (validación hacia adelante anidada con la regla de un error estándar, y la fórmula de Bayes Stein de Jorion) que se corrió con tasas de relleno constantes, por lo que sus resultados no se publican y quedan pendientes.

## Qué aprendí

* Con restricciones, el óptimo puede quedar en el límite y no donde la derivada es cero: es una solución de esquina. Para dos activos, el peso de varianza mínima en el activo tranquilo es (σ₂ al cuadrado menos ρσ₁σ₂) entre (σ₁ al cuadrado más σ₂ al cuadrado menos 2ρσ₁σ₂), y pasa de 100% cuando la correlación supera la volatilidad baja entre la alta.
* Una esquina tiene un costo medible. Con correlación de 0.8 y volatilidades de 10% y 20%, la solución libre (venta en corto del activo volátil) tiene volatilidad de 8.94%, y la restricción de no vender corto la deja en 10%.
* La prueba de la esquina: un activo con peso cero está bien en cero si su covarianza con la cartera es mayor que la varianza de la cartera, porque agregarlo subiría el riesgo. Con tres activos, el tercero tiene covarianza de 0.01415 contra una varianza de 0.00815, y los otros dos tienen exactamente 0.00815.
* Con seis pares contra el dólar, la mínima varianza libre puso pesos de 0.081 en USD/MXN, 0.217 en USD/JPY, 0.317 en USD/EUR, menos 0.129 en USD/GBP, 0.519 en USD/CAD y menos 0.005 en USD/BRL. Al prohibir las ventas en corto, 2 de 6 pesos quedaron en cero (USD/GBP y USD/BRL), y USD/CAD retuvo 0.485. El peso de USD/BRL ya era casi cero sin restricción, y el que de verdad cambió con la restricción fue USD/GBP.
* Los optimizadores concentran: casi la mitad del portafolio de mínima varianza quedó en USD/CAD. Un tope de 40% por activo lo recorta, y el resto se reparte entre los otros pares.
* En la muestra de entrenamiento, la restricción de no vender corto costó muy poco (volatilidad de 5.7% libre contra 5.8% solo largo).
* Fuera de muestra, las tres carteras de mínima varianza tuvieron volatilidad realizada de 5.4% a 5.5%, contra 5.8% de inverso de la volatilidad y 6.1% de pesos iguales. Es una sola partición, así que la ventaja de unos 0.7 puntos porcentuales sobre pesos iguales es una pista y no una prueba.
* Con 6 activos y unos 4.4 años de entrenamiento, el error de estimación de la covarianza no hizo que la mínima varianza saliera peor que las carteras simples. El problema de los optimizadores que amplifican el error de estimación se espera con muchos activos y poca historia, y esa situación no se probó aquí.
* El cociente entre la volatilidad fuera y dentro de muestra salió menor que 1 en las cinco carteras (de 0.91 a 0.95), porque el periodo de prueba fue más tranquilo que el de entrenamiento. Por eso el cociente no mide por sí solo la calidad de una cartera, y lo que se compara es la volatilidad de cada una en el mismo periodo.
* Sin restricciones, los pesos del portafolio de máximo Sharpe son proporcionales a la inversa de la covarianza por los rendimientos esperados. Con dos activos de Sharpe individual de 0.40 y correlación de 0.3, la combinación (2/3 y 1/3) llega a 0.496, porque la diversificación mejora el rendimiento por unidad de riesgo.
* El rendimiento esperado es el eslabón débil de la optimización. Con dos activos casi idénticos, un cambio de medio punto en el rendimiento esperado voltea los pesos de 4.8% y 95.2% a 95.2% y 4.8%, y el error estándar de un rendimiento promedio con 4 años de datos y 10% de volatilidad es de 5 puntos. Las varianzas y las correlaciones se estiman con mucha más precisión que los promedios, porque el promedio es chico frente a la volatilidad.
* Shrinkage jala lo estimado hacia algo más estable: la covarianza hacia una versión simple (Ledoit Wolf, con intensidad de 0.012 con 6 activos y unos 4.4 años, así que casi no cambió nada) y los rendimientos esperados hacia su promedio (con lambda de 1 no se encoge y con lambda de 0 todos son iguales, en cuyo caso el máximo Sharpe es la mínima varianza).
* En la simulación con parámetros verdaderos conocidos no hubo un método ganador en todos los casos. Con activos de Sharpe verdadero parecido, pesos iguales (0.385) quedó casi en el máximo posible (0.401) y le ganó al máximo Sharpe con estimaciones (0.324). Con activos muy distintos, el máximo Sharpe con estimaciones (0.798) le ganó a pesos iguales (0.575). La mínima varianza quedó por debajo en ambos (0.317 y 0.414) porque optimiza riesgo y no Sharpe, y encoger los rendimientos mejoró el mal caso en el primer escenario (percentil 10 de 0.296 contra 0.265) y costó en el segundo (0.693 contra 0.798).
* Con los seis pares reales, el máximo Sharpe sin encoger puso 93.5% en USD/JPY, el par con mayor rendimiento estimado (9.55% anual), y pasó de un Sharpe de 1.03 dentro de muestra a 0.01 fuera, con la mayor volatilidad fuera (8.4%). Con los rendimientos encogidos (lambda de 0.25), el portafolio se diversificó (0.439 en USD/JPY, 0.374 en USD/CAD y 0.160 en USD/EUR) con Sharpe de 0.887 dentro y menos 0.159 fuera, así que en esta partición el encogimiento no mejoró el resultado fuera de muestra frente al portafolio sin encoger.
* Las cuatro carteras (todas largo dólares contra otras monedas, sin carry) tuvieron Sharpe fuera de muestra de 0.01 o menos (menos 0.61 en pesos iguales, menos 0.29 en mínima varianza), mientras que dentro de muestra pesos iguales tenía 0.45. El rendimiento de estar largo dólares cambió de signo entre el entrenamiento y la prueba, y con un error estándar del Sharpe de cerca de 0.7, las diferencias entre carteras no se distinguen de ruido.
* El lambda no se elige mirando el periodo de prueba. Las opciones son una validación hacia adelante dentro del entrenamiento con la regla de un error estándar (el lambda más encogido que no se queda atrás del mejor), o una fórmula como la de Bayes Stein de Jorion, que lo calcula de los datos. El código está en el notebook, y los resultados con tasas reales quedan pendientes.
* En divisas, una posición larga en dólares contra otra moneda se financia sola y su rendimiento total es el precio más el diferencial de tasas (carry), que ya es un rendimiento en exceso. Los pesos que suman 1 y la restricción de solo largo son una formulación de activos como acciones, y para posiciones de FX que no cuestan nada la formulación natural es largo y corto, con pesos que no suman 1 y una volatilidad objetivo.

## Conclusión de la clase 20

El portafolio de varianza mínima se obtiene igualando la derivada a cero, pero cuando hay restricciones como la prohibición de vender corto o un tope por activo, el óptimo puede quedar pegado al límite, y eso es una solución de esquina. Con dos activos de volatilidades de 10% y 20%, el óptimo sin restricción rebasa 100% en el activo tranquilo cuando la correlación supera 0.5, y la restricción lo deja en la esquina con una volatilidad de 10% contra 8.94% de la solución libre. Con seis pares de divisas contra el dólar, la mínima varianza concentró casi la mitad en USD/CAD, el optimizador quería vender corto USD/GBP, y al prohibirlo 2 de 6 pesos quedaron en cero, con un costo muy pequeño en la muestra de entrenamiento. Fuera de muestra, las carteras de mínima varianza tuvieron una volatilidad realizada de 5.4% a 5.5% contra 6.1% de pesos iguales, una diferencia que viene de una sola partición y de solo 6 activos. Los optimizadores concentran y pueden amplificar el error de estimación de la covarianza, así que se validan fuera de muestra y se comparan contra carteras simples.

## Conclusión de la clase 21

El portafolio de máximo Sharpe combina rendimientos esperados y covarianza, y su punto débil es que los rendimientos esperados se estiman con mucho más error que la covarianza: con dos activos casi idénticos, medio punto de diferencia en el rendimiento esperado voltea los pesos de 4.8% y 95.2% a 95.2% y 4.8%, y con 4 años de datos el error estándar del rendimiento promedio es de unos 5 puntos. En una simulación con parámetros verdaderos conocidos, pesos iguales superó a los optimizadores cuando los activos eran parecidos, y el máximo Sharpe con estimaciones superó a pesos iguales cuando eran muy distintos, así que ningún método gana siempre. Con seis pares de divisas contra el dólar, el máximo Sharpe sin encoger puso 93.5% en USD/JPY y pasó de un Sharpe de 1.03 dentro de muestra a 0.01 fuera de muestra. Encoger los rendimientos diversificó el portafolio pero no mejoró el resultado fuera de muestra en esta partición, y las cuatro carteras tuvieron un Sharpe de 0.01 o menos fuera de muestra porque el rendimiento de estar largo dólares cambió de signo entre los dos periodos, con un error estándar de cerca de 0.7 que no permite distinguir entre ellas. El rendimiento esperado de un par de divisas medido solo con el precio es un insumo muy ruidoso para optimizar, y en FX el carry y una formulación de posiciones largas y cortas que se financian solas son pendientes antes de sacar conclusiones.

## Resultados de la clase 20

Dos activos con volatilidades de 10% y 20%, peso del activo tranquilo que minimiza la varianza:

| Correlación | Peso óptimo sin restricción |
|---|---|
| 0.0 | 0.80 |
| 0.3 | 0.895 |
| 0.5 | 1.00 |
| 0.8 | 1.333 |

Con correlación de 0.8, la volatilidad es de 8.94% con el peso libre de 1.333 y de 10% en la esquina de solo largo (peso de 1.0).

Tres activos con volatilidades de 10%, 15% y 20% y correlaciones de 0.2, 0.8 y 0.3 (entre 1 y 2, 1 y 3, y 2 y 3):

| Portafolio | Peso 1 | Peso 2 | Peso 3 | Volatilidad |
|---|---|---|---|---|
| Sin restricción | 1.069 | 0.264 | menos 0.333 | 7.84% |
| Solo largo | 0.736 | 0.264 | 0 | 9.03% |

Covarianza de cada activo con la cartera solo largo: 0.00815, 0.00815 y 0.01415, con una varianza de la cartera de 0.00815.

Seis pares contra el dólar (USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL), 1570 días de rendimientos diarios, con covarianza estimada con los primeros 1099 días y volatilidad realizada medida en los últimos 471. Un peso positivo significa estar largo dólares contra esa moneda. Pesos de cada cartera:

| Par | Pesos iguales | Inverso de la volatilidad | Mínima varianza libre | Mínima varianza solo largo | Solo largo, tope de 40% |
|---|---|---|---|---|---|
| USD/MXN | 0.167 | 0.135 | 0.081 | 0.073 | 0.090 |
| USD/JPY | 0.167 | 0.162 | 0.217 | 0.206 | 0.218 |
| USD/EUR | 0.167 | 0.206 | 0.317 | 0.235 | 0.287 |
| USD/GBP | 0.167 | 0.174 | menos 0.129 | 0.000 | 0.000 |
| USD/CAD | 0.167 | 0.224 | 0.519 | 0.485 | 0.400 |
| USD/BRL | 0.167 | 0.100 | menos 0.005 | 0.000 | 0.004 |

En la cartera de mínima varianza solo largo, 2 de 6 pesos quedaron en cero.

Volatilidad anualizada dentro y fuera de muestra:

| Cartera | Volatilidad dentro | Volatilidad fuera | Cociente fuera entre dentro |
|---|---|---|---|
| Pesos iguales | 6.7% | 6.1% | 0.909 |
| Inverso de la volatilidad | 6.3% | 5.8% | 0.917 |
| Mínima varianza libre | 5.7% | 5.4% | 0.946 |
| Mínima varianza solo largo | 5.8% | 5.4% | 0.929 |
| Solo largo, tope de 40% | 5.8% | 5.5% | 0.949 |

## Resultados de la clase 21

Dos activos con rendimientos esperados de 4% y 8%, volatilidades de 10% y 20% y correlación de 0.3 (tasa libre de 0). Cada uno por separado tiene Sharpe de 0.40. Frontera según el peso en el primer activo:

| Peso en el activo 1 | Rendimiento | Volatilidad | Sharpe |
|---|---|---|---|
| 0.0 | 8.0% | 20.0% | 0.400 |
| 0.2 | 7.2% | 16.7% | 0.431 |
| 0.4 | 6.4% | 13.7% | 0.466 |
| 0.6 | 5.6% | 11.4% | 0.493 |
| 0.8 | 4.8% | 10.0% | 0.482 |
| 1.0 | 4.0% | 10.0% | 0.400 |

El portafolio de máximo Sharpe tiene pesos de 0.667 y 0.333, rendimiento de 5.33%, volatilidad de 10.75% y Sharpe de 0.496.

Sensibilidad con dos activos de volatilidad de 10% y correlación de 0.9:

| Rendimientos esperados | Peso en el activo 1 | Peso en el activo 2 |
|---|---|---|
| 5.0% y 5.5% | 4.8% | 95.2% |
| 5.5% y 5.0% | 95.2% | 4.8% |

El error estándar del rendimiento promedio con 4 años de datos y volatilidad de 10% es de 5 puntos.

Simulación de seis activos (volatilidades de 8% a 15%, correlación de 0.5, 300 repeticiones, 1000 días). Sharpe verdadero promedio de cada método y, entre paréntesis, su percentil 10:

| Método | A: activos con Sharpe verdadero parecido | B: activos con Sharpe verdadero muy distinto |
|---|---|---|
| Mejor cartera posible (parámetros conocidos) | 0.401 | 0.945 |
| Pesos iguales | 0.385 (0.385) | 0.575 (0.575) |
| Mínima varianza | 0.317 (0.310) | 0.414 (0.371) |
| Máximo Sharpe con estimaciones | 0.324 (0.265) | 0.798 (0.606) |
| Máximo Sharpe encogido (lambda de 0.25) | 0.324 (0.296) | 0.693 (0.458) |

Seis pares contra el dólar (USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL), 1570 días del 8 de octubre de 2020 al 21 de octubre de 2026, con 1099 días de entrenamiento y 471 de prueba. Rendimiento y covarianza estimados solo con el entrenamiento, sin carry (solo precio). Intensidad del encogimiento de Ledoit Wolf: 0.012.

Rendimiento anual esperado estimado en el entrenamiento: USD/MXN menos 1.04%, USD/JPY 9.55%, USD/EUR 3.03%, USD/GBP 1.00%, USD/CAD 1.97% y USD/BRL 3.48%.

Pesos de cada cartera:

| Par | Pesos iguales | Mínima varianza (cov. encogida) | Máximo Sharpe (sin encoger) | Máximo Sharpe (encogido) |
|---|---|---|---|---|
| USD/MXN | 0.167 | 0.077 | 0.000 | 0.000 |
| USD/JPY | 0.167 | 0.208 | 0.935 | 0.439 |
| USD/EUR | 0.167 | 0.239 | 0.000 | 0.160 |
| USD/GBP | 0.167 | 0.000 | 0.000 | 0.000 |
| USD/CAD | 0.167 | 0.476 | 0.007 | 0.374 |
| USD/BRL | 0.167 | 0.000 | 0.058 | 0.028 |

Desempeño dentro y fuera de muestra:

| Cartera | Sharpe dentro | Rendimiento fuera | Volatilidad fuera | Sharpe fuera |
|---|---|---|---|---|
| Pesos iguales | 0.451 | menos 3.68% | 6.05% | menos 0.608 |
| Mínima varianza (cov. encogida) | 0.616 | menos 1.58% | 5.39% | menos 0.292 |
| Máximo Sharpe (sin encoger) | 1.028 | 0.10% | 8.42% | 0.012 |
| Máximo Sharpe (encogido) | 0.887 | menos 0.94% | 5.94% | menos 0.159 |

El error estándar aproximado del Sharpe fuera de muestra, con 471 días (cerca de 1.9 años), es de unos 0.73, así que las diferencias entre carteras no se distinguen de ruido.

## Limitaciones

* En la clase 20, los ejemplos de dos y tres activos usan volatilidades y correlaciones supuestas, así que no dicen nada sobre mercados. Con datos reales solo se minimiza la varianza, sin considerar rendimientos esperados, y la covarianza se estima con error. Los portafolios son de dólar contra seis divisas, un ejemplo didáctico muy cargado a un solo factor (el dólar, clase 12), y no una cartera lista para operar. Hay una sola partición de 70% y 30%, el periodo de prueba (471 días, cerca de 1.9 años) es más tranquilo que el de entrenamiento, y las diferencias de 0.1 a 0.7 puntos porcentuales de volatilidad entre carteras no se probaron estadísticamente. Los pesos son fijos, con rebalanceo diario implícito y sin costos de operación ni rotación. Con solo 6 activos, el error de estimación de la covarianza pesa poco, y los problemas de los optimizadores con muchos activos no se probaron. Los pesos dependen de la ventana de entrenamiento y cambiarían con otra, y la comparación de carteras no incluye una técnica de encogimiento de la covarianza. Los pesos que el solver deja en cero se redondean con una tolerancia de 0.000001.
* En la clase 21, los rendimientos de los pares son solo de precio y no incluyen el carry (diferencial de tasas), que en divisas es una parte grande del rendimiento de una posición. Las carteras de los resultados con datos reales son solo largo en dólares y con pesos que suman 1, una formulación pensada para activos como acciones, y para posiciones de FX que se financian solas lo natural es largo y corto con pesos que no suman 1 y una volatilidad objetivo, lo cual no se probó. Hay una sola partición de 70% y 30%, el periodo de prueba es corto (cerca de 1.9 años, con error estándar del Sharpe de unos 0.73) y las diferencias entre las cuatro carteras no se distinguen de ruido. El rendimiento de estar largo dólares cambió de signo entre el entrenamiento y la prueba, así que los rendimientos esperados estimados con el pasado no se mantuvieron. La lambda de 0.25 la elegí yo sin criterio y no se validó, y la elección de lambda queda pendiente. La simulación usa seis activos con correlación igual de 0.5, rendimientos normales e independientes en el tiempo y solo dos escenarios elegidos por mí, así que su conclusión (que ningún método gana siempre) depende de esa construcción. La tasa libre se tomó como 0 y no hay costos de operación. Las partes de carry y de elección de lambda del notebook se corrieron con tasas de relleno constantes (USD y MXN con los valores actuales y las demás en 0.0), por lo que sus resultados no se publican. La fórmula de Bayes Stein de Jorion del notebook se implementó de memoria y debe verificarse contra la referencia antes de citarla. Los pesos que el solver deja en cero se redondean con una tolerancia de 0.000001.
