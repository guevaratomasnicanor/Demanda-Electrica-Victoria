# ⚡ Demanda y Precio de la Electricidad en Victoria (Australia)

Análisis de la evolución de la **demanda** y el **precio de la electricidad** en el estado de Victoria (Australia) durante el período **2018–2023**, con énfasis en estacionalidad, patrones horarios y modelos de pronóstico.

---

## 🎯 Objetivo

- Analizar el comportamiento histórico de la demanda y el precio eléctrico.
- Identificar patrones estacionales y horarios.
- Evaluar la relación entre demanda y precio.
- Comparar modelos econométricos tradicionales con modelos de *machine learning* para pronóstico.

---

## 📁 Dataset

- **Ubicación:** Victoria, Australia  
- **Período:** 2018 – 2023  
- **Frecuencia:** Horaria (agregada a nivel mensual para forecasting)
- **Variables principales:**
  - Demanda eléctrica
  - Precio spot de la electricidad

---

## 📊 Análisis Exploratorio

### 🔌 Demanda Eléctrica

<img width="1361" height="699" alt="Captura de pantalla 2025-12-19 104553" src="https://github.com/user-attachments/assets/21acfb02-993e-470e-9612-8a3f31c809ac" />


- Los **picos máximos de demanda** se concentran en **verano**, asociados a olas de calor y mayor uso de refrigeración.
- La **demanda promedio mensual** es más elevada en **invierno**, reflejando un consumo más estable y sostenido.

---

### 💰 Precio de la Electricidad

<img width="1361" height="698" alt="Captura de pantalla 2025-12-19 104944" src="https://github.com/user-attachments/assets/87f646bc-448c-496f-810b-a57eb2a7328d" />


- Los **picos de precios** ocurren tanto en **verano** como en **invierno**.
- Se observan **precios negativos**, típicos de escenarios con alta generación renovable y baja demanda.
- El precio presenta una **estabilidad relativa**, interrumpida por shocks relevantes.
- El evento más significativo ocurre en el **invierno de 2022**, explicado por:
  - Conflicto Rusia–Ucrania
  - Fallas estructurales del sistema energético
  - Temperaturas inusualmente bajas

![Precio por hora](https://github.com/user-attachments/assets/bb27a168-12eb-49fa-8429-14b940ae1ca2)

- **Picos horarios de precio**:
  - Mañana en invierno
  - Entre 16 y 19 hs en verano

---

## 🔮 Modelos de Pronóstico

![Forecast](https://github.com/user-attachments/assets/34837ab8-cf13-46dc-8c14-9302cd369ca8)

### 📈 Pronóstico de Demanda — ARIMA

Se utilizó un modelo:

**ARIMA (1,0,0)(0,1,1)[12] con drift**

**Justificación:**
- Componente autoregresivo: la demanda pasada influye en la actual.
- Diferenciación estacional: captura patrones anuales.
- Media móvil: corrige errores de predicción.

**Métricas (training set):**

| Métrica | Valor |
|------|------|
| MAPE | 1.99% |
| RMSE | 137.08 |
| MAE | 96.23 |
| ACF1 | -0.01 |

**Conclusiones:**
- Modelo robusto y estable.
- Error promedio cercano al 2%.
- No se detecta autocorrelación en los residuos.
- Leve sesgo de sobreestimación.

---

### 📉 Pronóstico de Precio — ARIMA

Modelo evaluado:

**ARIMA (1,0,0)(0,0,1)[12]**

| Métrica | Valor |
|------|------|
| MAPE | 31.35% |
| RMSE | 40.35 |
| MAE | 24.08 |

**Limitaciones:**
- Alta volatilidad del precio.
- Errores grandes ocasionales (RMSE >> MAE).
- Tendencia a sobreestimar el precio.

---

### 🚀 Pronóstico de Precio — XGBoost (Modelo Final)

Debido al bajo desempeño de ARIMA, se implementó **XGBoost** con *feature engineering* avanzado:

- Variables cíclicas (meses)
- Lags de 1 y 12 meses
- Variables exógenas (ratio precio–demanda)
- Tendencias implícitas

**Métricas:**

| Métrica | Valor |
|------|------|
| MAPE | 12.56% |
| RMSE | 12.67 |
| MAE | 9.62 |
| MASE | 0.38 |

![XGBoost Forecast](https://github.com/user-attachments/assets/e8890e78-11a7-4e4b-8a7f-8be5c6db88ce)

**Resultados:**
- Reducción del error promedio del 31% al 12%.
- Modelo prácticamente insesgado.
- Persistencia de estructura en los residuos (oportunidad de mejora futura).

---

## 🔍 Key Insights

- 📈 **Correlación demanda–precio:** 59%
- 📉 **Demanda:** caída promedio anual del **0.83%**
- 📉 **Precio:** caída promedio anual del **4.29%**
- ⚡ El precio presenta mayor volatilidad que la demanda, justificando el uso de modelos no lineales

