
# Chatbot

El modelo construido es una red neuronal basada en LSTM (Long Short-Term Memory) diseñada para generar respuestas en un chatbot a partir de un diálogo de entrada.

## 1. Entrada y Preprocesamiento:
Dataset: El modelo se entrena con un conjunto de datos que contiene pares de diálogos (pregunta-respuesta). Cada línea en el archivo dialogs.txt representa un par de diálogos, separados por un tabulador (\t).
Tokenización: Las entradas y respuestas se convierten en secuencias numéricas mediante un Tokenizer. Este proceso asigna un número entero a cada palabra en el vocabulario.
Padding: Las secuencias se rellenan (padding) para que todas tengan la misma longitud, lo cual es necesario para poder procesarlas en la red neuronal.
## 2. Arquitectura del Modelo:
### Capa de Embedding:
Esta capa convierte las secuencias numéricas de palabras en vectores densos de      dimensión 128. Los vectores de embedding capturan relaciones semánticas entre palabras.

### Primera Capa LSTM:
La LSTM es una variante avanzada de redes neuronales recurrentes (RNN) que es capaz de capturar dependencias a largo plazo en las secuencias de datos. En esta capa, se configura return_sequences=True para que la salida sea una secuencia completa, donde cada palabra de la entrada produce una salida.
### Dropout:
Esta capa aplica una técnica de regularización para evitar el sobreajuste. Un porcentaje del 50% de las neuronas se apaga aleatoriamente durante el entrenamiento, lo que mejora la generalización del modelo.
### Segunda Capa LSTM:
Similar a la primera, pero ahora captura las dependencias de alto nivel en la secuencia de entrada procesada por la primera LSTM.
### Segunda Capa Dropout:
Otra capa de Dropout para continuar evitando el sobreajuste en la segunda capa LSTM.
### Capa Dense:
Esta capa totalmente conectada utiliza la activación softmax para generar probabilidades para cada palabra en el vocabulario en cada paso de la secuencia. El tamaño de salida de esta capa es igual al tamaño del vocabulario.
Esta capa genera una distribución de probabilidad sobre todo el vocabulario para cada palabra en la secuencia de salida, es decir, predice qué palabra debe aparecer en cada paso de la secuencia de respuesta.
## 3. Entrenamiento:
### Pérdida (categorical_crossentropy):
La función de pérdida calcula la discrepancia entre la secuencia de palabras predichas y la secuencia de palabras verdaderas (one-hot encoded). Esta pérdida se minimiza durante el entrenamiento para mejorar la precisión del modelo.
### Optimizador (adam):
El optimizador Adam se utiliza para ajustar los pesos del modelo, es una versión mejorada del descenso de gradiente estocástico que se adapta al aprendizaje en cada paso.
### CallBacks:
#### ReduceLROnPlateau: 
Este callback reduce la tasa de aprendizaje si la pérdida de validación no mejora después de ciertas épocas, lo que ayuda al modelo a converger mejor.
#### ModelCheckpoint: 
Este callback guarda el mejor modelo basado en la pérdida de validación, lo que asegura que el modelo final sea el más efectivo encontrado durante el entrenamiento.
## 4. Generación de Respuestas:
Una vez entrenado, el modelo toma una frase de entrada, la tokeniza y la convierte en una secuencia numérica. Luego, el modelo predice una secuencia de salida, que se convierte de nuevo en palabras utilizando el tokenizer.
El proceso genera una respuesta de texto basada en el diálogo de entrada.
## 5. Mejoras Introducidas:
Arquitectura Mejorada: Se incrementó la capacidad del modelo añadiendo capas LSTM y Dropout adicionales para mejorar la captura de patrones complejos en los datos y evitar el sobreajuste.
Regularización: Se introdujo Dropout para evitar el sobreajuste durante el entrenamiento.
Optimizador Avanzado: El uso de Adam permite un ajuste más eficiente y dinámico de los pesos durante el entrenamiento.