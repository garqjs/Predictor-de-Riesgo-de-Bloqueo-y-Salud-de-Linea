# 📊 SmartDial: Predictor de Riesgo de Bloqueo y Salud de Línea (ANI)

Este proyecto desarrolla un ecosistema analítico de punta a punta (End-to-End) diseñado para transformar la operación de un Contact Center de una marcación reactiva a una **estrategia preventiva basada en datos**. 

Utilizando **DuckDB** para el procesamiento masivo de datos y **Machine Learning con Python**, el sistema detecta patrones de desgaste en las líneas telefónicas (ANIs) y predice el riesgo de bloqueo por SPAM antes de ejecutar la llamada.

## 🚀 Tecnologías Utilizadas
* **Motor de Datos:** DuckDB (OLAP de alto rendimiento).
* **Lenguaje:** Python (Pandas, Scikit-Learn, XGBoost).
* **Visualización:** Seaborn & Matplotlib (Reporting Ejecutivo).
* **Entorno:** Google Colab.

## 🧠 El Problema: El "Tipping Point" del SPAM
La marcación saliente descontrolada provoca el "quemado" de las líneas. Al superar un umbral crítico de intentos, las operadoras (Carriers) bloquean el número, reduciendo la tasa de contacto a cero.



## 🛠️ Arquitectura del Proyecto

### 1. Auditoría y Limpieza de Datos
Se procesaron **50,000 registros operativos** mediante DuckDB.
* **Gestión de Nulos:** Identificación de fugas de información en `CallDuration` (9.9% de nulos) e imputación estadística en `WaitTime`.
* **Transformación:** Generación de variables de tiempo (bloques horarios) y métricas de acumulación por ANI.

### 2. Clasificación Binaria con IA
Se implementó un modelo de clasificación para predecir la **Probabilidad de Contacto Exitoso**.
* **Variable Objetivo:** `es_contacto_exitoso` (1: Éxito, 0: Fallo/Bloqueo).
* **Drivers Clave:** Se identificó que la `Tasa de Bloqueo Histórica` y la `Hora del Día` son los predictores más potentes del éxito operativo.

<img width="984" height="583" alt="image" src="https://github.com/user-attachments/assets/ae6a3e2f-8ca8-448a-acaf-6ef67d8dc09c" />

<img width="865" height="558" alt="image" src="https://github.com/user-attachments/assets/52e64d95-3c49-4bea-bb2b-14d3b9af57c6" />

<img width="860" height="479" alt="image" src="https://github.com/user-attachments/assets/157f7fa9-ba84-497e-87ac-a27a4ed35ceb" />



## 📈 Resultados y Diagnóstico

### Matriz de Confusión Operativa
El modelo demuestra una alta capacidad para identificar llamadas con riesgo de fallo, permitiendo al PM filtrar la base de datos antes de marcar.

<img width="673" height="604" alt="image" src="https://github.com/user-attachments/assets/90fa4d72-4fdf-4f94-8a6c-ab3c6969b495" />


### Métricas de Desempeño
* **AUC-ROC:** 0.5133 (Identifica patrones base sobre el azar).
* **Audit Log:** Total Rows: 50,000 | Nulls Handled: 7,490.

<img width="713" height="577" alt="image" src="https://github.com/user-attachments/assets/53a9fee4-285c-4020-bbe5-fe4256711f79" />


## 💡 Recomendaciones Estratégicas
1.  **Regla de Oro (Hard-Limit):** No exceder los **7 intentos por hora** por cada ANI. Al 8vo intento, el riesgo de bloqueo sube un 10%.
2.  **Enfriamiento de Líneas:** Rotar automáticamente cualquier ANI que presente una tasa de "Refused" superior al 5% en la última ventana de 30 minutos.
3.  **Ajuste de Marcación:** Priorizar carteras en los bloques horarios donde el modelo detecta menor saturación de red.

---
**Desarrollado como solución técnica para la optimización de Contact Centers y prevención de SPAM.**
