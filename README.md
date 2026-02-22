# 🎵 Predicción de Popularidad Musical: Optimización Coarse-to-Fine y Stacking Regressor

Este repositorio contiene el proyecto final de Aprendizaje Supervisado, donde se aborda el reto de predecir la popularidad de canciones utilizando sus características técnicas de audio. El proyecto destaca por el uso de **Pipelines robustos**, una estrategia de optimización jerárquica y el ensamblaje de modelos mediante **Stacking**.

---

## 📊 Descripción del Proyecto

El objetivo es determinar la variable `popularity` (0-100) basándose en atributos como *danceability*, *energy*, *loudness*, entre otros. Se trabaja con el **Song Popularity Dataset** de Kaggle.

### Puntos Clave del Desarrollo:
* **Anti-Leakage:** Uso estricto de `Pipeline` para garantizar que el escalado de datos se realice únicamente dentro de la validación cruzada.
* **Optimización Jerárquica:** Implementación de búsqueda **Coarse-to-Fine** para hiperparámetros en escala logarítmica.
* **Ensemble Learning:** Combinación de diversos modelos (Lineales, KNN, Árboles, SVM) mediante un **StackingRegressor**.

---

## 🛠️ Metodología y Fases

### 1. Análisis y Preprocesamiento
* Análisis de correlación y limpieza de características no relevantes.
* Escalado de variables numéricas con `MinMaxScaler` y gestión de nulos.
* División de datos en `train` y `test` con semilla fija para reproducibilidad.

### 2. Optimización Coarse-to-Fine
Para evitar búsquedas exhaustivas ineficientes, se aplicó un proceso de sintonización en tres niveles:
1. **Fase Coarse:** Exploración de órdenes de magnitud ($10^{-6}$ a $10^{6}$) para localizar la región de menor error.
2. **Fase Refinada:** Búsqueda con mayor densidad dentro del intervalo óptimo detectado.
3. **Ajuste Fino:** Sintonización lineal alrededor del mejor valor absoluto.

### 3. Modelos Implementados
Se optimizaron y compararon los siguientes algoritmos mediante `GridSearchCV`:
* **Regresión Lineal & Kernel Ridge**
* **K-Neighbors Regressor**
* **Decision Tree & Random Forest Regressor**
* **SVR (Support Vector Regression)**

### 4. Meta-Modelo (Stacking)
Se construyó un **StackingRegressor** utilizando los mejores modelos individuales como *base learners* y una **Regresión Lineal** como *meta-modelo*. Este enfoque permite que el algoritmo final aprenda a ponderar las predicciones de cada modelo según su fiabilidad relativa.

---

## 📈 Resultados
El modelo final se evaluó utilizando:
* **MAE** (Mean Absolute Error)
* **RMSE** (Root Mean Squared Error)

| Modelo | MAE (Test) | RMSE (Test) |
| :--- | :---: | :---: |
| Linear Regression | - | - |
| Random Forest | - | - |
| **Stacking Regressor** | **Mejor** | **Mejor** |

*(Nota: Rellenar con tus resultados finales)*

---

## 🎯 Conclusiones e Interpretación
* **Complementariedad:** El meta-modelo de Stacking reveló una mayor confianza en modelos de tipo [Mencionar tu modelo ganador], logrando una generalización superior a cualquier modelo individual.
* **Linealidad:** Se observó que el problema presenta componentes [lineales/no lineales] significativos, lo que justifica el uso de kernels complejos.
* **Limitaciones:** La popularidad es un fenómeno multivariable; las características de audio proporcionan una base sólida, pero factores externos (marketing, tendencias) actúan como ruido en el dataset.

---

## 🚀 Próximos Pasos (Roadmap)
Este proyecto forma parte de un itinerario de especialización en Machine Learning Aplicado:
- [x] Regresión Avanzada y Ensembles (Este proyecto).
- [ ] Predicción de Series Temporales (Time Series Analysis).
- [ ] **Desarrollo de Bot de Trading Algorítmico:** Aplicación de arquitecturas de Stacking y optimización para la predicción de activos financieros en tiempo real.

---

## 📦 Requisitos
```bash
pip install numpy pandas scikit-learn matplotlib seaborn
