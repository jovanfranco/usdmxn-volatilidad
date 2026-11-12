# USD/MXN y fundamentos cuantitativos con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance) y practica los fundamentos de probabilidad, estadística, álgebra lineal, cálculo, regresión, series de tiempo, modelos de volatilidad, control de pruebas múltiples, validación hacia adelante y optimización de portafolios que hay detrás.

Cada clase es un notebook de Python (Google Colab). Este README es un resumen; el detalle de cada bloque (qué aprendí, resultados y limitaciones) está en la carpeta `docs/`.

## Cómo está organizado

| Bloque | Clases | Temas | Detalle |
|---|---|---|---|
| 1 | 1 a 5 | Rendimientos, volatilidad, colas gordas y cruce de promedios | [docs/01_volatilidad_y_estrategias.md](docs/01_volatilidad_y_estrategias.md) |
| 2 | 6 a 10 | Probabilidad, Bayes, distribuciones, pruebas de hipótesis y problemas de entrevista | [docs/02_probabilidad_e_inferencia.md](docs/02_probabilidad_e_inferencia.md) |
| 3 | 11 a 16 | Álgebra lineal, PCA, cálculo, regresión y series de tiempo | [docs/03_algebra_calculo_regresion_series.md](docs/03_algebra_calculo_regresion_series.md) |
| 4 | 17 a 19 | EWMA y GARCH, pruebas múltiples y validación hacia adelante | [docs/04_volatilidad_y_validacion.md](docs/04_volatilidad_y_validacion.md) |
| 5 | 20 | Portafolios de mínima varianza y soluciones de esquina | [docs/05_portafolios_y_optimizacion.md](docs/05_portafolios_y_optimizacion.md) |

## Notebooks

| Clase | Notebook | Tema |
|---|---|---|
| 1 | `clase1_usdmxn_volatilidad.ipynb` | Rendimientos, volatilidad y anualización |
| 2 | `clase2_funciones_ciclos.ipynb` | Funciones, ciclos y tasa libre de riesgo |
| 3 | `clase3_graficas_distribucion.ipynb` | Gráficas, volatilidad móvil y dimensionamiento por volatilidad |
| 4 | `clase4_normal_colas_gordas.ipynb` | Normal, curtosis y colas gordas |
| 5 | `clase5_cruce_promedios.ipynb` | Cruce de promedios móviles con costos |
| 6 | `clase6_esperanza_varianza.ipynb` | Esperanza y varianza (didáctica) |
| 7 | `clase7_condicional_bayes.ipynb` | Probabilidad condicional y Bayes (didáctica) |
| 8 | `clase8_distribuciones_tcl.ipynb` | Binomial, ley de los grandes números y TCL (didáctica) |
| 9 | `clase9_hipotesis_confianza.ipynb` | Pruebas de hipótesis e intervalos de confianza |
| 10 | `clase10_probabilidad_entrevistas.ipynb` | Problemas de probabilidad de entrevista (didáctica) |
| 11 | `clase11_algebra_lineal.ipynb` | Portafolios con álgebra lineal |
| 12 | `clase12_valores_propios_pca.ipynb` | Valores propios y PCA |
| 13 | `clase13_calculo_optimizacion.ipynb` | Derivadas, varianza mínima y gradiente descendente |
| 14 | `clase14_regresion_lineal.ipynb` | Regresión lineal y errores robustos |
| 15 | `clase15_regresion_multiple_regularizacion.ipynb` | Regresión múltiple, ridge y lasso |
| 16 | `clase16_series_de_tiempo.ipynb` | Estacionariedad, ACF, PACF y ARMA |
| 17 | `clase17_ewma_garch.ipynb` | EWMA, GARCH y GJR |
| 18 | `clase18_pruebas_multiples.ipynb` | Pruebas múltiples y falsos positivos |
| 19 | `clase19_walk_forward.ipynb` | Validación hacia adelante (walk forward) |
| 20 | `clase20_portafolios_esquina.ipynb` | Portafolios de mínima varianza con restricciones |

## Lo más importante que encontré

* Los rendimientos de USD/MXN tienen colas gordas: 16 días a más de 3 desviaciones contra 3.4 esperados si fuera normal (curtosis en exceso de 3.81).
* Más de la mitad del movimiento conjunto de USD/MXN, USD/JPY y USD/EUR es un solo factor dólar (56.9% en el primer componente del PCA).
* La volatilidad se agrupa en rachas y un GARCH(1,1) con t de Student las captura: los residuos estandarizados al cuadrado ya no muestran autocorrelación (Ljung Box, p de 0.55). Fuera de muestra, GARCH tuvo el mejor QLIKE, aunque su ventaja sobre EWMA es pequeña.
* No encontré una estrategia de cruce de promedios con ventaja demostrable. Con una familia de 30 cruces, ninguna pasó el 5% ni siquiera sin corregir por pruebas múltiples.
* En la validación hacia adelante, elegir con el Sharpe pasado no le ganó al promedio de la familia: con 12 pliegues, la elegida pasó de un Sharpe de 1.15 dentro de muestra a menos 0.06 fuera, contra 0.19 del promedio de las 30.
* Con seis pares de divisas contra el dólar, un portafolio de mínima varianza tuvo volatilidad realizada fuera de muestra de 5.4%, contra 6.1% de pesos iguales, pero concentró casi la mitad en USD/CAD, y al prohibir las ventas en corto 2 de 6 pesos quedaron en cero (solución de esquina). Es una sola partición, así que la diferencia es una pista y no una prueba.

## Cómo correr los notebooks

1. Abre el notebook en Google Colab.
2. Si usa `arch` (clase 17), agrega `!pip install arch` en la primera celda.
3. Usa "Reiniciar sesión y ejecutar todo" para evitar celdas fuera de orden.
4. Los datos se descargan de Yahoo Finance con `yfinance`, así que los números pueden cambiar un poco si Yahoo corrige datos históricos.

## Limitaciones generales

* No hay una estrategia con ventaja demostrada. Los resultados son de aprendizaje.
* Las pruebas fuera de muestra son de una sola partición o de pocas ventanas, y los periodos de prueba son cortos, así que el Sharpe y las volatilidades tienen un margen de error grande.
* Casi todas las pruebas de las primeras clases no están corregidas por pruebas múltiples.
* Las clases 6, 7, 8 y 10 son ejercicios didácticos sin datos de mercado.
* Las limitaciones detalladas de cada clase están en los documentos de `docs/`.

## Próximos pasos

Markowitz completo con rendimientos esperados y encogimiento de la covarianza (shrinkage), y backtesting más sólido con rebalanceo periódico y costos.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
