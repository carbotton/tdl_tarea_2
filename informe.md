## Training RNN

# Weight and Biases

## Análisis GRU W and B
| **Modelo**            | **hidden_dim** | **n_layers** | **Bidireccional** | **LR**    | **Accuracy** | **Macro F1** | **Observaciones**                                                                 |
| :-------------------- | :------------- | :----------- | :---------------- | :-------- | :----------- | :----------- | :-------------------------------------------------------------------------------- |
| GRU (W&B – mejor run) | 128            | 1            | No                | 0.0064    | 0.980        | 0.89         | Alto rendimiento general, pero menor capacidad para clases minoritarias.          |
| GRU (W&B run #2)      | 32             | 3            | No                | 0.0117    | 0.974        | 0.85         | Mayor profundidad no introdece mejoras, esto puede estar contra-arrestado para el hecho de tener solo 32dim en la capa ocultas|
| GRU (W&B run #3)      | 128            | 1            | No                | 0.0098    | 0.973        | 0.85         | Opuesto al caso anterior, mayor capa ocualta pero sin suficiente profundidad |
| GRU (W&B run #4)      | 128            | 1            | No                | 0.0449    | 0.827        | 0.18         | Entrenamiento inestable generado seguramente por la tasa de aprendizaje alta.                             |
| GRU (W&B run #5)      | 32             | 1            | No                | 0.0266    | 0.934        | 0.56         | Arquitectura demasiado chica, con baja profundidas y una baja dimension de capa oculta|
| **GRU (original)**    | **64**         | **2**        | **Sí**            | **0.001** | **≈ 0.98**   | **0.92**     | Mejor equilibrio entre precisión y recall; detecta mejor las clases minoritarias. |

Todas las arquitecturas probadas en W&B son más pequeñas que la GRU original, ya sea por tener una menor profundidad (n_layers) o un hidden dim más reducido.
Ninguna de estas configuraciones logró superar los resultados obtenidos por nuestra arquitectura base.

Dado que los experimentos de W&B no incluyeron modelos más grandes o complejos, no es posible afirmar que nuestra arquitectura sea la mejor.
Sin embargo, los resultados muestran que la GRU original genera un excelente equilibrio entre capacidad de representación y rendimiento, motivo por el cual decidimos mantenerla como la configuración final.


## Análisis W&B LSTM

| **Modelo**            | **hidden_dim** | **n_layers** | **LR** | **Accuracy** | **Macro F1** | **Observaciones**                                                                          |
| :-------------------------------- | :------------- | :----------- | :----- | :----------- | :----------- |  :----------- |
| **LSTM (W&B run #1)** | 128            | 3            | 0.0079 | 0.907        | 0.49         | Aumento en el tamaño de la arquitectura genera peores resultados   |
| **LSTM (W&B run #2)** | 128            | 1            | 0.0029 | 0.983        | 0.90         | Balance entre hideen dims y profundida genera un resultado tan bueno como el de la arquitectura base |
| **LSTM (W&B run #3)** | 32             | 3            | 0.0065 | 0.969        | 0.84         | Aumentar la profundidad sigue geerando resultados por debajo de lo esperado aunque se haya intentendo balancear con un menor hidden dim |
| **LSTM (W&B run #4)** | 128            | 3            | 0.0033 | 0.970        | 0.86         | Buen desempeño global; *label smoothing* ayudó a suavizar el aprendizaje, pero sin superar al modelo base. |
| **LSTM (W&B run #5)** | 32             | 1            | 0.0075 | 0.979        | 0.89         | Modelo liviano, estable y con resultados cercanos al mejor run, aunque con menor capacidad representativa. |
| **LSTM (original)**   | 64             | 2            | 0.001  | ≈0.984       | 0.91         | Arquitectura equilibrada; destaca por su estabilidad y mejor detección de clases minoritarias.             |


Los resultados obtenidos en W&B muestran que las distintas configuraciones de LSTM alcanzan rendimientos altos, pero ninguna logra superar a la arquitectura original.
Las variantes más simples (con menor número de capas o hidden_dim reducido) mantienen una buena accuracy, aunque con un menor macro F1, lo que indica un desempeño más limitado en las clases minoritarias.

Se observa además una tendencia en la que las arquitecturas más grandes, especialmente las de mayor profundidad, obtienen resultados por debajo de lo esperado y presentan un costo computacional más alto.

La LSTM original alcanza el mejor equilibrio gracias a una configuración intermedia entre hidden_dim y n_layers, que ofrece buena capacidad de representación sin caer en sobreajuste.
Por este motivo, se mantiene como la arquitectura final seleccionada.