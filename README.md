⚡ Demanda y Precio de la Electricidad en Victoria (Australia)

Este proyecto analiza el comportamiento de la demanda y el precio de la electricidad en el estado de Victoria (Australia) durante el período 2018–2023.
El objetivo es comprender la dinámica entre ambas variables clave del sector energético y evaluar distintos enfoques de modelado y pronóstico.
# Análisis de la demanda
<img width="640" height="356" alt="Captura de pantalla 2025-12-16 161242" src="https://github.com/user-attachments/assets/600974fa-6e6f-4db3-9ae1-fa1885886364" />

Los picos máximos de demanda se observan durante el verano, principalmente asociados a temperaturas extremas y mayor uso de sistemas de refrigeración. 

<img width="636" height="360" alt="Captura de pantalla 2025-12-16 161332" src="https://github.com/user-attachments/assets/f8850339-9a6f-4677-a78d-cc31b31559cd" />

Sin embargo, al analizar la demanda promedio mensual, se observa que esta suele ser más elevada en invierno, reflejando un consumo más sostenido debido a calefacción y menor variabilidad climática diaria.

# Análisis del precio


<img width="626" height="345" alt="Captura de pantalla 2025-12-16 155607" src="https://github.com/user-attachments/assets/8ed491ba-64a0-4533-a220-07cd639a6922" />

Los picos de precios se concentran tanto en verano como en invierno.
Además, se observan precios negativos, típicos de días con alta generación renovable (solar y eólica) combinada con baja demanda..

<img width="631" height="359" alt="Captura de pantalla 2025-12-16 161344" src="https://github.com/user-attachments/assets/3c332600-a388-4d16-9c16-3f621674b38c" />

El precio promedio muestra una estabilidad relativa, interrumpida por algunos shocks significativos.
El más relevante ocurre en el invierno de 2022, explicado por:

El conflicto Rusia–Ucrania

Fallas estructurales en el sistema energético

Un invierno particularmente frío

<img width="643" height="361" alt="Captura de pantalla 2025-12-17 152744" src="https://github.com/user-attachments/assets/bb27a168-12eb-49fa-8429-14b940ae1ca2" />

Los picos horarios de precio suelen darse:

Por la mañana en invierno

Entre 16 y 19 horas en verano


# Pronóstico de demanda y precio: 
<img width="630" height="360" alt="Captura de pantalla 2025-12-18 170912" src="https://github.com/user-attachments/assets/34837ab8-cf13-46dc-8c14-9302cd369ca8" />

📈 Pronóstico de Demanda (ARIMA)

Para modelar la demanda se utilizaron métodos tradicionales de econometría, en particular un:

ARIMA (1,0,0)(0,1,1)[12] con drift

Justificación del modelo:

Componente autoregresivo: la demanda pasada influye en la actual, dado que las temperaturas no cambian bruscamente.

Diferenciación estacional: permite capturar la similitud entre el mismo mes de distintos años.

Componente de media móvil: corrige errores de predicción.

Métricas del modelo (training set):

Métrica	Valor
MAPE	1.99%
RMSE	137.08
MAE	96.23
ACF1	-0.01

Conclusiones:

El modelo es robusto, con un error promedio cercano al 2%.

RMSE y MAE similares indican ausencia de errores extremos.

No hay autocorrelación en los residuos.

Se detecta un leve sesgo de sobreestimación de la demanda.

Las métricas implican que el modelo es robusto, con una falla promedio del 2% en las predicciones de demanda. El RMSE no es drásticamente superior al MAE, lo que nos indica que no se estan cometiendo errores catastróficos en la predicción. El ACF es casi 0 lo que implica queno hay autocorrelación en los residuos. Existe un leve sesgo de sobreestimación de la demanda. 


📉 Pronóstico de Precio

El modelado del precio resultó más complejo debido a su alta volatilidad.

ARIMA

El mejor modelo ARIMA probado fue:

ARIMA (1,0,0)(0,0,1)[12]

Métrica	Valor
MAPE	31.35%
RMSE	40.35
MAE	24.08

Problemas detectados:

Error promedio elevado (~31%).

RMSE casi el doble del MAE, indicando errores grandes ocasionales.

Tendencia a sobreestimar los precios.

Se probaron modelos alternativos (ETS, transformaciones logarítmicas), pero solo se obtuvieron mejoras marginales.

🚀 XGBoost (Modelo Final de Precio)

Finalmente, se implementó un modelo XGBoost con feature engineering avanzado, incluyendo:

Componentes cíclicos (ej. diciembre y enero como meses cercanos).

Lags de 1 y 12 meses.

Variables exógenas (ratio precio–demanda).

Tendencias y estacionalidades implícitas.

Métricas del modelo:

Métrica	Valor
MAPE	12.56%
RMSE	12.67
MAE	9.62
MASE	0.38

                ME       RMSE       MAE       MPE      MAPE      MASE       ACF1
Training set 2.5031865 12.6696213 9.6220821 0.1931699 12.5553054 0.3845479 0.2667223
Se ve una mejora sustancial respecto al ARIMA: Reducción del error promedio de 31 a 12%, RMSE y MAE menores, insesgadez del modelo pero con información en los residuos. 

<img width="635" height="360" alt="Captura de pantalla 2025-12-18 195033" src="https://github.com/user-attachments/assets/e8890e78-11a7-4e4b-8a7f-8be5c6db88ce" />


Key Insights:

- La correlación entre demanda-precio es del 59%.
- La demanda tiene una disminución anual del 0,83% mientras que el precio decae un 4,29% anual.

  
