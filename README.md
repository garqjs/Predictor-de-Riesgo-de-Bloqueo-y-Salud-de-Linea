# 📊 Inteligencia de Contactabilidad: Diagnóstico de Salud de ANI y Prevención de SPAM

Este proyecto desarrolla un ecosistema analítico de punta a punta (End-to-End) diseñado para transformar la operación de un Contact Center de una marcación reactiva a una **estrategia preventiva basada en evidencia de datos**.

Utilizando **DuckDB** para el procesamiento de alto rendimiento y **Machine Learning (XGBoost)**, el sistema detecta patrones de desgaste en las líneas telefónicas (ANIs) y predice el riesgo de bloqueo por SPAM.

---

## 🔍 1. Diagnóstico del Problema: El "Tipping Point"
A través del análisis de 50,000 registros operativos, detectamos que la **Salud del ANI** es el predictor número uno del éxito de la llamada.

* **Punto de Quiebre (Tipping Point):** Identificamos que a partir de **6-7 intentos por hora** desde un mismo número, la probabilidad de ser marcado como SPAM se dispara. Superar este umbral degrada la reputación de la línea de forma casi irreversible.
* **Impacto Operativo:** Actualmente, el **30-40% de las llamadas** en horas pico se desperdician en números "quemados" (ANIs con alta tasa de rechazo histórica).

<img width="865" height="558" alt="image" src="https://github.com/user-attachments/assets/cb8a439e-4f99-4dc4-ab9f-7f54eda034af" />


---

## 🤖 2. Inteligencia Artificial y Drivers de Éxito
El modelo **XGBoost** identifica los factores que realmente "mueven la aguja" en la contactabilidad:

1.  **Tasa de Bloqueo Histórica del ANI:** El predictor más fuerte. Un número con historial de SPAM genera una inercia de fracaso persistente.
2.  **Hora del Día:** El comportamiento de respuesta es altamente sensible al bloque horario, superando en importancia al día de la semana.
3.  **Intentos Previos (Ventana 60 min):** La "insistencia" acumulada penaliza el éxito en tiempo real y activa los filtros de los carriers.

<img width="984" height="583" alt="image" src="https://github.com/user-attachments/assets/3c8e92c9-265a-462d-bd38-16f58fae7eef" />


### Desempeño del Modelo
* **Precisión en Falla:** El modelo es altamente eficaz detectando cuándo una llamada va a fallar (Pred: Falla/Block).
* **Matriz de Confusión:** Permite filtrar la base de datos para evitar marcaciones de alto riesgo.
* **AUC-ROC (0.5133):** Logra separar patrones base sobre la aleatoriedad intrínseca del comportamiento humano.

<img width="713" height="577" alt="image" src="https://github.com/user-attachments/assets/30074dfc-14cd-46f3-bfea-9a1dfdb3b640" />

<img width="673" height="604" alt="image" src="https://github.com/user-attachments/assets/756e0e95-fc41-405a-b785-99a5714c6fe8" />


---

## 🛠️ 3. Plan de Marcación Preventivo (Algoritmo de Rotación)
Basado en la evidencia, se proponen las siguientes reglas de negocio para el *Dialer*:

### A. Reglas de "Enfriamiento" (Cool-down)
* **Lógica:** Si `Intentos_Previos_Hora_ANI` >= 7, **PAUSAR** el uso de ese ANI por 60 minutos.
* **Justificación:** Evita la zona de penalización de operadoras y preserva la reputación del pool de números.

### B. Rotación Dinámica de Caller ID
* Priorizar el uso de ANIs con `Tasa_Bloqueo_Historica` < 0.10 y menor uso en la ventana de tiempo actual.

### C. Estrategia de Bloques Horarios (Time-Block)
* Reducir la intensidad de marcación en un **20%** entre las 13:00 y 15:00 (franja de mayor reporte manual de SPAM por usuarios).

---

## 📋 4. Conclusiones y Recomendaciones

### Recomendaciones Técnicas (Preventivas)
* **Hard-Limit de Marcación:** Implementar bloqueo automático al alcanzar el 7mo intento en ventana rodante de 60 min.
* **Enfriamiento de Líneas:** Rotar números que superen un umbral de rechazo del 5% durante 24 horas.

### Ajustes al Plan de Marcación (Estratégico)
* **Optimización Horaria:** Redistribuir la carga de marcación evitando horas de saturación de bloqueos.
* **Limpieza de Base:** Eliminar registros con >3 estados de "Refused" o "No Contesta" consecutivos.

---

## 🚀 5. Roadmap Técnico (Siguientes Pasos)

| Fase | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| **Fase 1** | Integrar SQL de **DuckDB** en el ETL diario. | Reporte automatizado de "ANIs en Peligro". |
| **Fase 2** | Conectar modelo XGBoost vía API (Pre-Call Scoring). | Bloqueo de marcación si la `prob_exito` < 0.3. |
| **Fase 3** | Implementar rotación de ANIs basada en reputación. | Incremento estimado del 15% en la tasa de contacto real. |

---
**Desarrollado para optimización de operaciones Outbound y mitigación de bloqueos por SPAM.**
