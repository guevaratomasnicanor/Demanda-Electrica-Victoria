# Demanda-Electrica-Victoria

# Análisis de la demanda
<img width="640" height="356" alt="Captura de pantalla 2025-12-16 161242" src="https://github.com/user-attachments/assets/600974fa-6e6f-4db3-9ae1-fa1885886364" />

Los picos mas altos de la demanda se ven en el verano.

<img width="636" height="360" alt="Captura de pantalla 2025-12-16 161332" src="https://github.com/user-attachments/assets/f8850339-9a6f-4677-a78d-cc31b31559cd" />

De todas maneras, la demanda promedio mensual suele ser mas alta en invierno.


# Análisis del precio


<img width="626" height="345" alt="Captura de pantalla 2025-12-16 155607" src="https://github.com/user-attachments/assets/8ed491ba-64a0-4533-a220-07cd639a6922" />

Los picos mas altos del precio se ven en verano e invierno. Pueden darse precios negativos en dias de sol, viento y poca demanda.

<img width="631" height="359" alt="Captura de pantalla 2025-12-16 161344" src="https://github.com/user-attachments/assets/3c332600-a388-4d16-9c16-3f621674b38c" />

El precio promedio tiene una estabilidad relativa con algunos shocks: El mas fuerte fue el del invierno del 2022, debido al conflicto entre Rusia-Ucrania, fallas estructurales y un invierno con bajas temperaturas.

<img width="643" height="361" alt="Captura de pantalla 2025-12-17 152744" src="https://github.com/user-attachments/assets/bb27a168-12eb-49fa-8429-14b940ae1ca2" />

Los picos de precio suelen darse a la mañana(invierno) y de 16 a 19(verano).


# Pronóstico de demanda y precio: 
<img width="630" height="360" alt="Captura de pantalla 2025-12-18 170912" src="https://github.com/user-attachments/assets/34837ab8-cf13-46dc-8c14-9302cd369ca8" />

Para predecir la demanda, se utilizaron métodos tradicionales de la econometría como el modelo ARIMA. Lo mas probable es que la demanda sea estacional, con una leve tendencia a la baja, por eso se eligió un ARIMA (1,0,0) (0,1,1) 12 con drift. El primer 1 es un componente autoregresivo ya que la demanda del pasado influye en el mes anterior(las temperaturas no cambian drasticamente). Se necesitó una diferenciación estacional para que el modelo tome en cuenta los valores del mismo mes pero del año anterior. Ademas tiene un componente de media movil que corrige segun los errores. 
                 ME     RMSE      MAE        MPE     MAPE      MASE        ACF1
Training set -1.374044 137.0758 96.22719 -0.1089891 1.990936 0.6593064 -0.01150183

Las métricas implican que el modelo es robusto, con una falla promedio del 2% en las predicciones de demanda. El RMSE no es drásticamente superior al MAE, lo que nos indica que no se estan cometiendo errores catastróficos en la predicción. El ACF es casi 0 lo que implica queno hay autocorrelación en los residuos. Existe un leve sesgo de sobreestimación de la demanda. 


Con el pronóstico de precio la tarea fue mas ardua, ya que el ARIMA nos devolvia metricas peores:
ARIMA(1,0,0)(0,0,1)[12] with non-zero mean 
                 ME      RMSE      MAE       MPE      MAPE     MASE       ACF1
Training set -0.4787373 40.34717 24.07731 -15.41171 31.35298 0.4263605 0.001628764 

El modelo se equivoca un 31% en promedio debido a la alta volatilidad en los precios. esto se confirma con que el RMSE es casi el doble que el MAE. El modelo tiende a predecir precios mas altos de los reales. 
Se intentó con otros métodos como ETS y log, pero solo generaban mejoras marginales. Finalmente, tuve exito con xgboost. 
Este es un modelo complejo con feature engeneering de componentes cíclicos( ejemplo que diciembre y enero estan cerca aunque sn mes 12 y 1), lags de 1 mes y 12 meses, variables exógenas como el ratio precio-demanda, etc. 
Con estas modificaciones conseguimos las siguientes métricas:
Metrica Valor

                ME       RMSE       MAE       MPE      MAPE      MASE       ACF1
Training set 2.5031865 12.6696213 9.6220821 0.1931699 12.5553054 0.3845479 0.2667223
Se ve una mejora sustancial respecto al ARIMA: Reducción del error promedio de 31 a 12%, RMSE y MAE menores, insesgadez del modelo pero con información en los residuos. 

<img width="635" height="360" alt="Captura de pantalla 2025-12-18 195033" src="https://github.com/user-attachments/assets/e8890e78-11a7-4e4b-8a7f-8be5c6db88ce" />


Key Insights:
- La correlación entre demanda-precio es del 59%.
- La demanda tiene una disminución anual del 0,83% mientras que el precio decae un 4,29% anual.
- 
  
