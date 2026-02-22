# Predicción de Popularidad Musical: Optimización Coarse-to-Fine y Stacking de Modelos

Este proyecto documenta el desarrollo de un sistema de regresión diseñado para predecir la popularidad de canciones basándose en sus atributos técnicos de audio. El flujo de trabajo abarca desde el análisis exploratorio hasta la implementación de un Stacking Regressor, demostrando un control exhaustivo sobre el sobreajuste y la optimización de hiperparámetros.

## Metodología y Estrategia de Optimización

El núcleo técnico de este trabajo se basa en dos pilares:

1. **Pipelines y prevención de fuga de datos**: Se ha utilizado la clase `Pipeline` de scikit-learn para encapsular el escalado de datos (`MinMaxScaler`) y el entrenamiento. Esto garantiza que no exista *data leakage* y que el preprocesamiento se valide de forma independiente en cada split de la validación cruzada.
2. **Optimización Coarse-to-Fine**: En lugar de realizar búsquedas aleatorias, se ha implementado una estrategia jerárquica para los parámetros en escala logarítmica (como Alpha, Gamma o C). Primero se exploraron órdenes de magnitud amplios para identificar la región de interés y, posteriormente, se realizaron búsquedas densas para encontrar el valor óptimo.



## Modelos Evaluados

Se han entrenado y ajustado mediante `GridSearchCV` los siguientes algoritmos:

* Regresión Lineal y Kernel Ridge.
* K-Neighbors Regressor (KNN).
* Árboles de Decisión y Random Forest.
* Support Vector Regression (SVR).

## Análisis del Modelo de Stacking

Para obtener la máxima capacidad predictiva, se implementó un `StackingRegressor` utilizando los modelos anteriores como *base-learners* y una Regresión Lineal como meta-modelo. El análisis de los coeficientes del meta-modelo arroja las siguientes conclusiones:

### Jerarquía de importancia
El meta-modelo confía principalmente en un tándem de dos enfoques: el **Random Forest (peso: 0.63)** y el **K-Neighbors (peso: 0.49)**. Juntos dominan la toma de decisiones, combinando reglas de decisión lógica con similitud por distancia.

### Modelos descartados por redundancia
El Árbol de Decisión individual recibió un peso casi nulo (**-0.001**). Esto confirma que, al estar el Random Forest presente (que ya es una agrupación de árboles), un árbol solitario no aporta información incremental. El SVR también fue ignorado significativamente (**-0.06**).

### El papel de los coeficientes negativos
El Kernel Ridge presenta un coeficiente de **-0.20**. En un ensamble de este tipo, los valores negativos actúan como mecanismos de corrección: si el resto de modelos tienden a sobreestimar la popularidad, el meta-modelo utiliza este valor para compensar y ajustar el resultado final.

### Mejora en la precisión
La implementación del Stacking ha demostrado ser efectiva, reduciendo el error **RMSE de 17.23** (mejor modelo individual) a **16.88**, lo que confirma que la combinación de perspectivas mejora la generalización.



## Análisis Crítico y Conclusiones

* **Naturaleza del problema**: Los resultados indican que la relación entre el audio y la popularidad es eminentemente **no lineal**. Los modelos lineales puros se vieron superados por enfoques capaces de capturar patrones complejos, como Random Forest.
* **Sobreajuste y Generalización**: El modelo que mostró mayor varianza fue el Árbol de Decisión, con una clara tendencia a memorizar los datos. El Stacking ha logrado mitigar este efecto, convirtiéndose en el modelo con mejor capacidad de generalización en el conjunto de test.
* **Limitaciones y realismo**: Es importante destacar que predecir el éxito de una canción basándose solo en el audio es un desafío limitado. Factores externos críticos como el peso de la discográfica, la inversión en marketing, la fama previa del artista o la viralidad en plataformas sociales no están presentes en este dataset, lo que establece un "techo" natural para la precisión del modelo.
