# Bloque 2: Probabilidad e inferencia (clases 6 a 10)

[Volver al README](../README.md)

## Qué hace

* Resuelve problemas de esperanza y varianza y los comprueba con simulaciones Monte Carlo.
* Calcula probabilidades condicionales y aplica el teorema de Bayes con un ejemplo ilustrativo.
* Simula la distribución binomial, la ley de los grandes números y el teorema central del límite.
* Aplica pruebas de hipótesis, p-valores e intervalos de confianza, y calcula el estadístico t de un rendimiento promedio de USD/MXN.
* Resuelve problemas clásicos de probabilidad de entrevista (martingalas, ruina del apostador, análisis del primer paso, Monty Hall, cumpleaños y coleccionista) y los comprueba con simulaciones.

## Contenido

* `clase6_esperanza_varianza.ipynb`: fundamentos de probabilidad (no usa datos de USD/MXN). Esperanza, varianza, simulación Monte Carlo de un dado y de una apuesta con ventaja pequeña.
* `clase7_condicional_bayes.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Probabilidad condicional, independencia y teorema de Bayes con un ejemplo ilustrativo de 100 días.
* `clase8_distribuciones_tcl.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Distribución binomial, regla de 68, 95 y 99.7, error estándar, ley de los grandes números y teorema central del límite con simulaciones.
* `clase9_hipotesis_confianza.ipynb`: pruebas de hipótesis y p-valor exacto con la binomial (60 de 100 aciertos da 2.8% y 55 de 100 da 18.4%). Intervalo de confianza del rendimiento diario de USD/MXN de octubre de 2020 a octubre de 2026: de menos 0.042% a 0.026%, que incluye el cero (estadístico t de menos 0.47). También calcula la probabilidad de falsos positivos al probar 20 estrategias (64.2%).
* `clase10_probabilidad_entrevistas.ipynb`: fundamentos de probabilidad (no usa datos de mercado). Simulaciones de ruina del apostador con y sin ventaja, tiempo esperado hasta ver un patrón de caras y cruces, problema del cumpleaños, Monty Hall y coleccionista de caras del dado, comparadas con su solución teórica.

## Qué aprendí

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

## Limitaciones

* Las clases 6, 7, 8 y 10 son ejercicios didácticos. Las simulaciones usan semilla fija y el ejemplo de noticias y movimientos de la clase 7 son 100 días hipotéticos, no datos reales de mercado.
* En la clase 9, el intervalo de confianza y el estadístico t suponen rendimientos independientes. Eso no se cumple del todo en USD/MXN por la volatilidad en rachas, así que son una aproximación.
* Los modelos de ruina de la clase 10 suponen apuestas independientes de tamaño fijo y probabilidad constante, algo que los mercados no cumplen de forma estricta.
