
# Laboratorio 2: Algoritmos de Optimización en Machine Learning

## Objetivo

Analizar el comportamiento de algoritmos de optimización y regularización en modelos de regresión lineal aplicados a un dataset real.

---

## Dataset utilizado

- Nombre: CarPrice_Assignment.csv  
- Fuente: Kaggle ([hellbuoy/car-price-prediction](https://www.kaggle.com/datasets/hellbuoy/car-price-prediction))  
- Muestras: 205 registros  
- Variable dependiente: `price`  
- Variables predictoras seleccionadas:
  - `engine-size`
  - `horsepower`
  - `curb-weight`

---

## Análisis exploratorio

Se utilizó la función `scatter_matrix` para observar visualmente las relaciones lineales entre las variables predictoras y el precio. A partir de la matriz de dispersión, se concluyó que:

- Todas las variables muestran una correlación positiva con el precio.
- `engine-size` tiene una fuerte relación lineal con el precio.
- `horsepower` también mantiene una tendencia lineal clara, aunque con algo de dispersión.
- `curb-weight` presenta una correlación visual más moderada pero consistente.

Este análisis respalda el uso de regresión lineal como técnica de modelado, ya que se cumple el supuesto de linealidad.

---

## Modelado con regresión lineal

Se entrenaron modelos con distintas proporciones de partición de datos (entrenamiento/prueba):

| Proporción | MSE           | R² Score |
|------------|----------------|----------|
| 70/30      | 14,790,927.38  | 0.7865   |
| 50/50      | 11,894,814.11  | 0.7873   |
| 40/60      | 12,395,386.79  | 0.7804   |

**Conclusión:** El modelo fue estable en desempeño. La proporción 50/50 ofreció el mejor resultado global, indicando que la selección de variables predictoras fue apropiada.

---

## Optimización con Stochastic Gradient Descent (SGD)

Se aplicó el algoritmo `SGDRegressor`, escalando previamente los datos con `StandardScaler`. Los resultados fueron:

- MSE: 14,883,073.58  
- R²: 0.7852

**Conclusión:** El desempeño fue muy similar al de la regresión lineal clásica. Aunque no se mejora la precisión, este optimizador es útil en escenarios con grandes volúmenes de datos o flujos continuos, ya que permite entrenamiento incremental.

---

## Regularización

Se compararon los modelos con Lasso (L1), Ridge (L2) y ElasticNet (L1+L2):

| Modelo       | MSE           | R²      | Coeficientes (aprox)       |
|--------------|----------------|---------|-----------------------------|
| Lasso (L1)   | 14,791,193.76  | 0.7865  | [3333, 1965, 2289]          |
| Ridge (L2)   | 14,809,215.55  | 0.7862  | [3292, 1977, 2298]          |
| ElasticNet   | 16,934,679.14  | 0.7556  | [2332, 1948, 2094]          |

**Conclusión:**  
- Lasso ofreció el mejor balance entre precisión y simplicidad.
- Ridge se comportó de forma similar pero penaliza menos los coeficientes.
- ElasticNet penalizó más fuertemente y redujo el desempeño general.

---

## Modelo final: Lasso

- Coeficientes:
  - `engine-size`: 3333.47
  - `horsepower`: 1965.05
  - `curb-weight`: 2289.25
- Intercepto: 13243.24
- MSE: 14,791,193.76
- R²: 0.7865

**Interpretación:**  
Cada incremento unitario en cualquiera de las variables predictoras (estandarizadas) se asocia con un aumento en el precio del automóvil. Todos los coeficientes son positivos, indicando que el impacto de estas variables sobre el precio es directo. `engine-size` resultó ser la variable más influyente.

---

## Conclusiones generales

Este laboratorio permitió aplicar y comparar distintos enfoques para optimizar modelos de regresión lineal. Se aprendió a:

- Realizar análisis exploratorio y seleccionar adecuadamente las variables predictoras.
- Evaluar el efecto de cambiar la proporción de datos de entrenamiento y prueba.
- Implementar un optimizador alternativo como `SGDRegressor` y analizar su comportamiento frente al enfoque estándar.
- Aplicar técnicas de regularización y evaluar su impacto sobre el modelo final y los coeficientes.

El modelo final elegido (Lasso) explica aproximadamente el 78.65% de la variabilidad en el precio del automóvil, lo cual representa un desempeño aceptable. Además, mantiene la interpretabilidad de los coeficientes y evita el sobreajuste. El enfoque metodológico seguido garantiza que el modelo no solo sea preciso, sino también robusto y explicable.
