# Análisis de Correlación y Regresión: Factores de Salud en la ENAHO

Este módulo del proyecto aplica técnicas de análisis estadístico avanzado (matrices de correlación y modelos de regresión) sobre los datos de la **Encuesta Nacional de Hogares (ENAHO)** del Perú. El objetivo es identificar, medir y validar la fuerza de las relaciones entre las características demográficas de la población, su nivel de acceso a la salud mediante seguros, y el autorreporte de condiciones crónicas.

## 1. Objetivos del Análisis

* **Objetivo General:**
    * Evaluar estadísticamente la fuerza, dirección y significancia de las relaciones entre las variables socio-demográficas y el autorreporte de enfermedades crónicas mediante modelos lineales y logísticos.
* **Objetivos Específicos:**
    * Construir y analizar matrices de correlación para identificar colinealidad entre los predictores (como los diferentes tipos de seguros y el nivel educativo).
    * Modelar el impacto cuantitativo de variables continuas (como la edad) y categóricas (como el género y la ubicación geográfica urbano/rural).
    * Aislar el efecto del "sesgo de diagnóstico" para demostrar formalmente cómo la cobertura de salud altera el registro estadístico de la morbilidad en el Perú.

## 2. Metodología Analítica

El cuaderno implementa un flujo de trabajo enfocado en la solidez estadística:

1. **Análisis de Correlación:** Uso de coeficientes de correlación (Pearson/Spearman según la naturaleza de la variable) para mapear el comportamiento conjunto de las variables e identificar posibles redundancias de información.
2. **Tratamiento de Variables:** Dummificación y codificación de factores categóricos estructurales (afiliaciones a EsSalud, SIS, Seguro Privado y estratos geográficos).
3. **Modelado de Regresión:** Ajuste de coeficientes para medir la probabilidad de ocurrencia del evento crónico, evaluando los pesos relativos mediante métricas de bondad de ajuste.

## 3. Estructura de Variables Evaluadas

| Dimensión | Variable | Tratamiento Estadístico | Descripción |
| :--- | :--- | :--- | :--- |
| **Target** | `cronico` | Binaria (0 / 1) | Presencia de enfermedad crónica autorreportada. |
| **Demográfica** | `edad` | Continua | Factor de riesgo biológico principal medido en años. |
| **Demográfica** | `genero` / `sexo` | Binaria codificada | Control de variabilidad biológica entre hombres y mujeres. |
| **Geográfica** | `estrato_urbano_rural` | Binaria dummificada | Control del entorno de residencia y densidad poblacional. |
| **Acceso Institucional** | `seguro_essalud` | Binaria dummificada | Impacto de la cobertura de seguridad social contributiva. |
| **Acceso Subsidiado** | `seguro_sis` | Binaria dummificada | Impacto del aseguramiento público para poblaciones vulnerables. |

## 4. Conclusiones Clave del Modelo

* **Asociación Directa de la Edad:** La edad muestra la correlación positiva más robusta y consistente con el autorreporte de enfermedades crónicas, consolidándose como el predictor biológico de mayor peso en el modelo de regresión.
* **El Sesgo del Aseguramiento (EsSalud vs. Sin Seguro):** El análisis de regresión confirma que la falta de seguro actúa como un falso factor protector debido a la falta de tamizaje. Por el contrario, la afiliación a **EsSalud** incrementa de manera significativa el logaritmo de la probabilidad de autorreporte, demostrando que el acceso institucional formal es una condición necesaria para que el dato clínico exista en la encuesta.
* **Efecto de la Urbanización:** Residir en áreas urbanas se correlaciona positivamente con mayores tasas de autorreporte crónico. Esto refleja tanto una exposición a estilos de vida urbanos como una mayor densidad de infraestructura médica disponible para el diagnóstico en comparación con el ámbito rural.

## 5. Conclusiones Estadísticas y Análisis de Negocio

El modelado predictivo y el análisis cuantitativo arrojaron métricas clave que permiten rechazar hipótesis preliminares y aislar patrones estructurales de los datos:

* **Efecto de Margen en el Aseguramiento (EsSalud vs. Sin Seguro):**
  El coeficiente de la Regresión Logística reveló un impacto crítico en los canales de atención: los individuos que pertenecen al régimen contributivo de **EsSalud** concentran la mayor probabilidad de diagnóstico. De manera simétrica, aquellos que carecen de esta cobertura específica reflejan un **decremento del 21.5% en la probabilidad** de reportar una enfermedad crónica. Esto demuestra empíricamente que la variable "Sin Seguro" actúa en el dataset como un falso factor protector; no implica ausencia de enfermedad, sino una **barrera de acceso al tamizaje clínico** que impide la existencia del registro estadístico (subregistro por sesgo de diagnóstico).

* **Rechazo de Hipótesis Educativa mediante Dummificación:**
  El análisis exploratorio inicial sugería que el nivel de instrucción general tenía una correlación lineal débil con la salud crónica. Sin embargo, al aplicar la técnica de *dummificación* (aislando categorías excluyentes), el modelo determinó que la **Educación Básica Especial** se vincula estrechamente a condiciones de comorbilidad severas. Este comportamiento no lineal valida la importancia de desagregar variables categóricas complejas antes del modelado predictivo para evitar la pérdida de señales analíticas críticas.

* **Interacción Demográfica-Espacial Coherente:**
  La edad (como variable continua) demostró el gradiente de riesgo biológico más estable y con mayor significancia estadística ($p$-value). Al cruzarse con el estrato geográfico, se identificó que la residencia en **áreas urbanas** incrementa exponencialmente el autorreporte en comparación con el ámbito rural. Este fenómeno responde a dos factores medibles en la ENAHO: una mayor exposición a transiciones epidemiológicas (estilos de vida urbanos) y una densidad drásticamente superior de infraestructura médica y puntos de contacto con el sistema de salud, facilitando la detección oportuna.

* **Robustez del Clasificador de Regresión:**
  La evaluación del modelo mediante la curva ROC y la métrica AUC confirma la viabilidad del clasificador lineal para estimar la probabilidad del autorreporte de comorbilidades. La matriz de confusión evidencia que, utilizando únicamente características demográficas y de afiliación de seguros de la ENAHO, es posible segmentar a la población objetivo con niveles óptimos de precisión y *recall*, sentando las bases para algoritmos predictivos de asignación de presupuestos en salud pública.
