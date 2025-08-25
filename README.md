# Predicción de la Supervivencia en el Titanic – k-Vecinos Más Cercanos (KNN) en R

Clasificación de los pasajeros que sobrevivieron al naufragio del Titanic mediante un modelo KNN construido con **tidyverse** y **caret**.

## Conjunto de datos

**Fuente:** Kaggle – *Titanic: Machine Learning from Disaster*  
**Enlace:** <https://www.kaggle.com/competitions/titanic/overview>

## Resumen del proyecto

El célebre desafío Titanic de Kaggle plantea una tarea de aprendizaje automático sencilla pero didáctica: **predecir qué pasajeros sobrevivieron al naufragio de 1912**.  
En este repositorio abordamos el problema con un clasificador **k-Vecinos Más Cercanos (KNN)** implementado en R. El flujo de trabajo comprende:

- **Análisis exploratorio de datos (EDA)** y visualización.  
- **Limpieza de datos e ingeniería de características** (p. ej. extracción de títulos de los pasajeros, tamaño de la familia, tamaño del grupo de billete).  
- **Preprocesamiento**: imputación, codificación *one-hot* y escalado de variables.  
- **Entrenamiento del modelo** y ajuste de hiperparámetros (*k*, tipo de ponderación de distancia) mediante validación cruzada.  
- **Evaluación** en un conjunto de prueba independiente y generación de la sumisión para Kaggle.  

El modelo KNN proporciona una línea base transparente que sirve como punto de partida para algoritmos más complejos.

## Metodología

1. **EDA** – distribuciones de edad, tarifa y clase; tasas de supervivencia por sexo y clase; mapa de correlaciones.  
2. **Ingeniería de características**  
   - `Title` extraído de `Name`; títulos poco frecuentes combinados.  
   - `FamilySize = SibSp + Parch + 1`.  
   - Indicador `IsAlone`; `TicketGroupSize` (pasajeros que comparten billete).  
   - Depuración y consolidación de niveles categóricos.  
3. **Preprocesamiento**  
   - Imputación de `Age` con el método *random forest* de **mice**.  
   - Relleno de valores faltantes en `Embarked` con **S** (moda).  
   - Escalado de las variables numéricas a media cero y varianza unitaria.  
4. **Modelo** – motor `kknn` de **caret** con validación cruzada de 10 pliegues; rejilla `k = 3 … 35` y comparación entre ponderación uniforme e inversa a la distancia.  
5. **Métricas** – Exactitud, Kappa, Precisión/Recall, F1 y ROC-AUC; se muestran la matriz de confusión y la curva ROC.  

## Resultados clave

| Métrica | Valor |
|---------|-------|
| Mejor **k** (CV) | **15** |
| Exactitud de validación | **83 %** |
| Puntaje público en Kaggle | **0.78468** |
| Principales predictores | `Sex = female`, `Fare > 50`, `Pclass = 1`, `IsAlone = 0` |

El modelo KNN supera al clasificador de mayoría (≈ 62 %), aunque todavía hay margen para métodos basados en árboles o *ensembles* que suelen superar el 85 %.

## Próximos pasos

- Comparar con **Regresión Logística**, **Random Forest** y **XGBoost**.  
- Realizar **selección de variables** o **PCA** para reducir la dimensionalidad.  
- Probar distintas métricas de distancia (Manhattan, Minkowski) y funciones *kernel* (triangular, epanechnikov).  
- Desplegar como una **aplicación Shiny** para predicción interactiva.  
