# Bloque 5: Portafolios y optimización (clase 20)

[Volver al README](../README.md)

## Qué hace

* Encuentra el portafolio de varianza mínima de dos activos con una fórmula cerrada y muestra la solución de esquina que aparece al prohibir las ventas en corto.
* Resuelve el portafolio de varianza mínima de N activos con álgebra lineal (sin restricciones) y con optimización numérica (solo largo y con tope por activo), y comprueba la esquina con la covarianza de cada activo contra la cartera.
* Compara cinco carteras de seis pares de divisas contra el dólar (pesos iguales, inverso de la volatilidad, mínima varianza libre, mínima varianza solo largo y solo largo con tope de 40%), estimando la covarianza con el 70% más antiguo de los datos y midiendo la volatilidad realizada en el 30% más reciente.

## Contenido

* `clase20_portafolios_esquina.ipynb`: optimización de portafolios con `numpy` y `scipy`. Ejemplos a mano con dos activos de volatilidades de 10% y 20%: el peso óptimo en el activo tranquilo es 0.80, 0.895, 1.0 y 1.333 con correlaciones de 0, 0.3, 0.5 y 0.8, el umbral de correlación es 0.5 (volatilidad baja entre volatilidad alta), y con correlación de 0.8 la solución libre (peso de 1.333, es decir 33% vendido corto en el otro activo) tiene volatilidad de 8.94% contra 10% en la esquina de solo largo. Con tres activos de volatilidades de 10%, 15% y 20%: sin restricción los pesos son 1.069, 0.264 y menos 0.333 (volatilidad de 7.84%), y solo largo son 0.736, 0.264 y 0 (volatilidad de 9.03%), con covarianzas de cada activo con la cartera de 0.00815, 0.00815 y 0.01415 contra una varianza de la cartera de 0.00815. Con datos reales de USD/MXN, USD/JPY, USD/EUR, USD/GBP, USD/CAD y USD/BRL (1570 días de rendimientos comunes de octubre de 2020 a octubre de 2026): cinco carteras entrenadas con 1099 días y evaluadas en los 471 días siguientes.

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

## Conclusión de la clase 20

El portafolio de varianza mínima se obtiene igualando la derivada a cero, pero cuando hay restricciones como la prohibición de vender corto o un tope por activo, el óptimo puede quedar pegado al límite, y eso es una solución de esquina. Con dos activos de volatilidades de 10% y 20%, el óptimo sin restricción rebasa 100% en el activo tranquilo cuando la correlación supera 0.5, y la restricción lo deja en la esquina con una volatilidad de 10% contra 8.94% de la solución libre. Con seis pares de divisas contra el dólar, la mínima varianza concentró casi la mitad en USD/CAD, el optimizador quería vender corto USD/GBP, y al prohibirlo 2 de 6 pesos quedaron en cero, con un costo muy pequeño en la muestra de entrenamiento. Fuera de muestra, las carteras de mínima varianza tuvieron una volatilidad realizada de 5.4% a 5.5% contra 6.1% de pesos iguales, una diferencia que viene de una sola partición y de solo 6 activos. Los optimizadores concentran y pueden amplificar el error de estimación de la covarianza, así que se validan fuera de muestra y se comparan contra carteras simples.

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

## Limitaciones

* En la clase 20, los ejemplos de dos y tres activos usan volatilidades y correlaciones supuestas, así que no dicen nada sobre mercados. Con datos reales solo se minimiza la varianza, sin considerar rendimientos esperados, y la covarianza se estima con error. Los portafolios son de dólar contra seis divisas, un ejemplo didáctico muy cargado a un solo factor (el dólar, clase 12), y no una cartera lista para operar. Hay una sola partición de 70% y 30%, el periodo de prueba (471 días, cerca de 1.9 años) es más tranquilo que el de entrenamiento, y las diferencias de 0.1 a 0.7 puntos porcentuales de volatilidad entre carteras no se probaron estadísticamente. Los pesos son fijos, con rebalanceo diario implícito y sin costos de operación ni rotación. Con solo 6 activos, el error de estimación de la covarianza pesa poco, y los problemas de los optimizadores con muchos activos no se probaron. Los pesos dependen de la ventana de entrenamiento y cambiarían con otra, y la comparación de carteras no incluye una técnica de encogimiento de la covarianza. Los pesos que el solver deja en cero se redondean con una tolerancia de 0.000001.
