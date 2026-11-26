# USD/MXN y fundamentos cuantitativos con Python

Proyecto de aprendizaje en finanzas cuantitativas. Analiza el tipo de cambio USD/MXN con datos públicos de mercado (Yahoo Finance) y practica los fundamentos de probabilidad, estadística, álgebra lineal, cálculo, regresión, series de tiempo, modelos de volatilidad, control de pruebas múltiples, validación hacia adelante, optimización de portafolios y construcción de señales que hay detrás.

Cada clase es un notebook de Python (Google Colab). Este README es un resumen; el detalle de cada bloque (qué aprendí, resultados y limitaciones) está en la carpeta `docs/`.

## Cómo está organizado

| Bloque | Clases | Temas | Detalle |
|---|---|---|---|
| 1 | 1 a 5 | Rendimientos, volatilidad, colas gordas y cruce de promedios | [docs/01_volatilidad_y_estrategias.md](docs/01_volatilidad_y_estrategias.md) |
| 2 | 6 a 10 | Probabilidad, Bayes, distribuciones, pruebas de hipótesis y problemas de entrevista | [docs/02_probabilidad_e_inferencia.md](docs/02_probabilidad_e_inferencia.md) |
| 3 | 11 a 16 | Álgebra lineal, PCA, cálculo, regresión y series de tiempo | [docs/03_algebra_calculo_regresion_series.md](docs/03_algebra_calculo_regresion_series.md) |
| 4 | 17 a 19 | EWMA y GARCH, pruebas múltiples y validación hacia adelante | [docs/04_volatilidad_y_validacion.md](docs/04_volatilidad_y_validacion.md) |
| 5 | 20 a 22 | Portafolios de mínima varianza, máximo Sharpe, shrinkage y posiciones largas y cortas con volatilidad objetivo | [docs/05_portafolios_y_optimizacion.md](docs/05_portafolios_y_optimizacion.md) |
| 6 | 23 a 27 | Señales sistemáticas: momentum, fuga de información, combinación de señales, carry en divisas, gestión de su riesgo y validación hacia adelante de sus reglas | [docs/06_senales_sistematicas_fx.md](docs/06_senales_sistematicas_fx.md) |

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
| 21 | `clase21_markowitz_shrinkage.ipynb` | Máximo Sharpe, error de estimación y shrinkage |
| 22 | `clase22_largo_corto_vol_objetivo.ipynb` | Posiciones largas y cortas con volatilidad objetivo |
| 23 | `clase23_momentum_fx.ipynb` | Momentum en divisas con costos |
| 24 | `clase24_fuga_y_combinar_senales.ipynb` | Fuga de información y combinación de señales |
| 25 | `clase25_carry_fx.ipynb` | Carry en divisas: diferencial de tasas |
| 26 | `clase26_gestion_riesgo_carry.ipynb` | Gestión de riesgo del carry: volatilidad objetivo y reducción tras caídas |
| 27 | `clase27_validacion_reglas_riesgo.ipynb` | Validación hacia adelante de las reglas de riesgo del carry |

## Lo más importante que encontré

* Los rendimientos de USD/MXN tienen colas gordas: 16 días a más de 3 desviaciones contra 3.4 esperados si fuera normal (curtosis en exceso de 3.81).
* Más de la mitad del movimiento conjunto de USD/MXN, USD/JPY y USD/EUR es un solo factor dólar (56.9% en el primer componente del PCA).
* La volatilidad se agrupa en rachas y un GARCH(1,1) con t de Student las captura: los residuos estandarizados al cuadrado ya no muestran autocorrelación (Ljung Box, p de 0.55). Fuera de muestra, GARCH tuvo el mejor QLIKE, aunque su ventaja sobre EWMA es pequeña.
* No encontré una estrategia de cruce de promedios con ventaja demostrable. Con una familia de 30 cruces, ninguna pasó el 5% ni siquiera sin corregir por pruebas múltiples.
* En la validación hacia adelante, elegir con el Sharpe pasado no le ganó al promedio de la familia: con 12 pliegues, la elegida pasó de un Sharpe de 1.15 dentro de muestra a menos 0.06 fuera, contra 0.19 del promedio de las 30.
* Con seis pares de divisas contra el dólar, un portafolio de mínima varianza tuvo volatilidad realizada fuera de muestra de 5.4%, contra 6.1% de pesos iguales, pero concentró casi la mitad en USD/CAD, y al prohibir las ventas en corto 2 de 6 pesos quedaron en cero (solución de esquina). Es una sola partición, así que la diferencia es una pista y no una prueba.
* Un optimizador de máximo Sharpe sin encoger los rendimientos puso 93.5% en USD/JPY y pasó de un Sharpe de 1.03 dentro de muestra a 0.01 fuera. Los rendimientos esperados se estiman con mucho más error que la covarianza, y en una simulación ningún método ganó siempre.
* Con posiciones largas y cortas y una volatilidad objetivo de 8%, la volatilidad objetivo se cumplió dentro de muestra y fuera quedó entre 7.3% y 7.6%. El Sharpe fuera de muestra de las cuatro carteras fue de menos 0.61 a 0.08, con un error estándar de 0.73, así que no hay evidencia de que una supere a otra, y el largo y corto usó 2.3 veces de exposición bruta.
* Una señal de momentum de precio (ventanas de 21 a 252 días, escalada por volatilidad) en los seis pares no mostró evidencia de ventaja entre 2021 y 2026: los Sharpe antes de costos fueron de menos 0.33 a 0.21 con un error estándar de 0.44, y los costos supuestos de 1 a 10 puntos base lo empeoraron. La referencia de siempre largo dólar tuvo un Sharpe de 0.50, también sin significancia. Con cinco años y seis pares, el experimento solo detecta ventajas grandes.
* Una fuga de información (usar la señal del mismo día) infla un resultado hasta volverlo irreconocible: con una señal de 5 días, el Sharpe pasó de menos 0.03 a 10.90, y una prueba de control con datos remuestreados dio 2.14 con trampa contra menos 0.02 con rezago. Combinar tres señales de precio (momentum de 126 días, reversión de 5 días y reversión de 504 días) redujo la volatilidad, pero no creó ventaja: ninguna señal fue significativa (error estándar del Sharpe de 0.49) y la combinada tuvo un Sharpe de menos 0.24.
* Al sumar el diferencial de tasas (carry) a los seis pares contra el dólar, una señal de carry (largo la moneda con tasa más alta) tuvo un Sharpe de 0.94 entre 2022 y 2026 (t de 1.93, p de 0.054 y 0.163 con Bonferroni), casi todo por intereses (3.74% anual contra 0.63% del precio). Fue la señal con mejor resultado del bloque, pero no alcanza significancia al corregir por pruebas múltiples, y tiene asimetría negativa (menos 0.77): muchos meses chicos a favor y pocos meses grandes en contra. Combinarla con el momentum la empeoró (Sharpe de 0.13), porque el momentum tuvo un Sharpe de menos 0.60.
* Dos reglas de riesgo sobre el carry redujeron las pérdidas grandes: apalancar según la volatilidad reciente de la cartera (objetivo de 4.5%) subió el Sharpe de 0.94 a 1.11 y llevó la asimetría de menos 0.77 a menos 0.27, y reducir a la mitad la exposición tras una caída de 3% bajó la caída máxima de menos 7.43% a menos 5.49% a cambio de rendimiento. La mejora del Sharpe de la primera no se distingue de cero (diferencia de 0.17, intervalo de 95% de menos 0.04 a 0.35), aunque un control con el apalancamiento mezclado al azar sí favorece el momento en que reduce el riesgo (1% del azar iguala el Sharpe real).
* Con una familia de 19 reglas de riesgo sobre el carry, las de volatilidad objetivo con reacción más rápida (lambda de 0.90) tuvieron el mejor Sharpe (1.18 contra 0.94 sin regla), pero con Bonferroni de 19 pruebas el p de la mejor es 0.295, y el mejor Sharpe de 19 estrategias sin ventaja ya promedia 0.89 por azar. En validación hacia adelante, elegir con el pasado escogió siempre la misma regla y obtuvo un Sharpe fuera de muestra de 1.09 contra 0.93 sin regla, una diferencia menor que el error estándar (0.56).

## Cómo correr los notebooks

1. Abre el notebook en Google Colab.
2. Si usa `arch` (clase 17), agrega `!pip install arch` en la primera celda.
3. Usa "Reiniciar sesión y ejecutar todo" para evitar celdas fuera de orden.
4. Los datos se descargan de Yahoo Finance con `yfinance`, así que los números pueden cambiar un poco si Yahoo corrige datos históricos.

## Limitaciones generales

* No hay una estrategia con ventaja demostrada. Los resultados son de aprendizaje.
* Las pruebas fuera de muestra son de una sola partición o de pocas ventanas, y los periodos de prueba son cortos, así que el Sharpe y las volatilidades tienen un margen de error grande.
* Casi todas las pruebas de las primeras clases no están corregidas por pruebas múltiples.
* Los rendimientos de las carteras de las clases 20 a 23 son solo de precio, sin carry (diferencial de tasas), que en divisas es una parte grande del rendimiento, y los costos de la clase 23 son un supuesto.
* Las clases 6, 7, 8 y 10 son ejercicios didácticos sin datos de mercado.
* Las limitaciones detalladas de cada clase están en los documentos de `docs/`.
* Los rendimientos de las carteras de las clases 20 a 24 son solo de precio, sin carry, y el carry de las clases 25 y 26 se aproxima con tasas de corto plazo mensuales de distintas fuentes (no con puntos forward). Los costos de las clases 23 a 26 son un supuesto.

## Próximos pasos

Medir el carry con puntos forward reales, probar otras señales (valor) junto con las de precio y el carry, usar los costos reales de operación y elegir el parámetro de encogimiento con validación hacia adelante.

## Aviso

Proyecto educativo con datos públicos. No es recomendación de inversión.
