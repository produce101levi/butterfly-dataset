# Clasificación con Machine Learning de Diferentes Tipos de Mariposas

## Abstract

El entrenamiento de modelos de Machine Learning para un conjunto de datos de identificación de especies de mariposa implica el conocimiento y la utilización de los parámetros e hiperparámetros necesarios para lograr su precisión. Es por ello que este reporte detalla la metodología utilizada para llegar a una mejora en el modelo con base en conocimientos adquiridos a partir de dos artículos científicos. Se emplea la augmentación de imágenes, la división de un conjunto de datos en sets de entrenamiento, validación y pruebas, y se emplea un modelo pre-entrenado con el fin de lograr una mayor precisión en el modelo que se desea entrenar (cross-training). El modelo pasó de presentar señales de underfitting (precisión del 55% en entrenamiento y 45% en validación), a un estado óptimo (~75% precisión en entrenamiento, validación y pruebas).

## Introducción

La clasificación de imágenes es esencial en el marco del Machine Learning. Se han llevado a cabo varios experimentos y metodologías para intentar lograr una mayor precisión en la misma, dentro de las cuales la Red Neuronal Convolucional (CNN por sus siglas en inglés) ha mostrado ser la más efectiva. 

El objetivo de este artículo es mostrar cómo los parámetros, hiperparámetros y capas en un modelo que emplea CNN pueden afectar significativamente el rendimiento y la precisión del mismo.

Se toma en consideración un dataset obtenido desde kaggle llamado **Butterfly Image Classification**. Este set contiene varias imágenes de mariposas que pueden ser clasificadas en 75 clases diferentes. 

<img width="1189" height="1790" alt="distributionClasses" src="https://github.com/user-attachments/assets/fa3f5b38-3328-4c51-8f94-3a52a64c1fb5" />


Este set contiene un set de entrenamiento y uno de pruebas. Existen dos CSV dentro del dataset; train.csv y test.csv. Sin embargo, el CSV de test no contiene las clasificaciones correctas de las imágenes, por lo que no es viable utilizarlo como set para probar que el modelo sea efectivo.

Es por ello que se decidió dividir el set originalmente destinado al entrenamiento en un set de entrenamiento, validación y pruebas. Del 70% dedicado al entrenamiento, se toma el 30% como el set de validación, el cual servirá como guía para saber si el modelo está siendo entrenado de forma correcta.

## Metodología

Para asegurar una mayor precisión en el modelo y una mayor cantidad de datos sobre los cuales se puede basar, se implementó la augmentación de datos. Dentro de la misma, se aplicaron los siguientes parámetros:

| Parámetro | Valor |
| --------- | ----- |
| Rotation Range | 10 |
| Width Shift Range | 0.2 |
| Height Shift Range | 0.2 |
| Shear Range | 0.3 |
| Zoom Range | 0.3 |
| Horizontal flip | True |

Al ser un conjunto de datos que cuenta con 75 diferentes clases, se decidió implementar cinco diferentes matrices de confusión en los resultados. En cada matriz, se observará la confusión entre 15 diferentes clases (para observar claramente los resultados).

### Modelo base
Inicialmente, el experimento se llevó a cabo con un modelo básico sin mejoras implementadas.

| Parámetro | Valor |
| --------- | ----- |
| Learning Rate | 1x10^-4 |
| Input shape | 64x64x3 |
| Batch Size | 8 |
| Número de clases | 75 |
| Número de imágenes de entrenamiento | 4549 |
| Número de imágenes de prueba | 1950 |
| Optimizer | RMSprop |
| Número de layers | 3 |
| Número de epochs | 250 |

Este modelo cuenta con cuatro layers como parte de su arquitectura. 

| Layers |
| ------ |
| Conv2D(activation='reLu') |
| Flatten |
| Dense(256, activation='reLu') |
| Dense(75, activation='softmax') |

Además, el tamaño de las imagenes fue cambiado de **224x224** a **64x64**, ya que un tamaño reducido beneficia la velocidad y precisión del modelo.

Utiliza la fórmula de Cross Enthropy para calcular el loss en cada epoch.

Los resultados obtenidos a partir del entrenamiento del modelo base fueron los siguientes:

#### Gráfico representando la evolución del valor accuracy en el set de entrenamiento
<img width="547" height="435" alt="téléchargement" src="https://github.com/user-attachments/assets/02452f11-3c92-4027-b932-63b50b594efe" />

En este gráfico se observa un claro signo de **underfitting**, en donde el porcentaje precisión del modelo se estanca en aproximadamente un 55% tras 250 iteraciones. Esto indica que, en el conjunto de datos correspondiente a las fotos de entrenamiento, alrededor del 55% de las mariposas son clasificadas correctamente. 

#### Gráfico representando la evolución del valor loss en el set de entrenamiento
<img width="547" height="435" alt="téléchargement (1)" src="https://github.com/user-attachments/assets/fa408417-ad5b-4e18-8aaa-0eb6337c6096" />

#### Gráfico representando la evolución del valor accuracy en el set de validación
<img width="556" height="435" alt="téléchargement (2)" src="https://github.com/user-attachments/assets/5acd1cf0-ab39-4f2f-9d31-4d30ea9a288e" />

El gráfico muestra un estancamiento en valores mucho más bajos a los del set de entrenamiento, quedando en alrededor del 42.5%. Tras 250 iteraciones se indica que, en el set de validaciones, únicamente el 42.5% de las mariposas (en promedio) son clasificadas correctamente.

#### Gráfico representando la evolución del valor loss en el set de validación
<img width="556" height="435" alt="téléchargement (3)" src="https://github.com/user-attachments/assets/63d3bc5d-8aa5-45c1-b01b-389ea282ac0c" />

#### Interpretación del modelo base
Este modelo tiene una arquitectura muy simple, contando con únicamente 4 layers. Es por ello que la cantidad de parámetros que toma el modelo de entrenamiento no es suficiente para entrenarlo lo suficiente para reconocer con claridad el tipo de mariposa a partir de una foto. Al ser un modelo con underfitting, se puede inferir que el valor de precisión al probar el modelo será inferior a lo esperado.

### Modelo mejorado

Posterior a la prueba del modelo base, se llevaron a cabo diferentes mejoras para lograr un mejor entrenamiento. Con base en los artículos científicos consultados, se tomó la decisión de implementar el modelo pre-entrenado **InceptionV3**, que es viene del modele GoogLeNet. En un artículo donde se emplearon 120 imágenes de mariposas para clasificarlas en 4 diferentes especies, se notó que el modelo CNN GoogLeNet tenía un mejor rendimiento que otros modelos pre-entrenados tales como AlexNet. Se denotó que GoogLeNet logra una alta precisión detectando patrones en las diferentes especies de hojas, logrando reconocer 36 tipos de hojas.

InceptionV3 tiene una arquitectura que consiste de 48 capas de profundidad. En este modelo, los parámetros utilizados fueron los siguientes:

| Parámetro | Valor |
| --------- | ----- |
| Tasa de Aprendizaje | 1x10^-6 |
| Input shape | 75x75x3 |
| Batch Size | 8 |
| Número de clases | 75 |
| Número de imágenes de entrenamiento | 4549 |
| Número de imágenes de prueba | 1950 |
| Optimizer | RMSprop |
| Número de layers | 48 (InceptionV3) |
| Número de epochs | 120 |

El tamaño de las imágenes fue cambiado de 64x64 a 75x75, ya que el modelo pre-entrenado InceptionV3 exige un mínimo de 75x75 como resolución. 

El batch size se toma basado en el que se seleccionó en los experimentos detallados dentro de los artículos científicos.

El valor de la tasa de aprendizaje fue cambiado a 1x10^-6, ya que con un valor menor existía una demostración clara de **overfitting*. En este caso, se mostraban picos notorios en los valores de pérdida de la validación, sumado a que la precisión en el modelo de entrenamiento fue ~80%, en validación ~65%, y en pruebas ~51%

Si bien en los artículos se utilizó el optimizador Adam, se notó que utilizando RMSprop no se presentó overfitting o pérdida excesiva en el conjunto de validación.

<img width="567" height="455" alt="comparison" src="https://github.com/user-attachments/assets/4db6960f-82b7-43ea-8f07-c3163ea7b72d" />


En una de las matrices de confusión que se generaron para esta iteración se evidencia las fallas que comete el modelo al identificar las imagenes.

<img width="654" height="606" alt="cm5" src="https://github.com/user-attachments/assets/630f9ef4-fe4f-4de3-bbe9-ab9186afe6ae" />


Con una tasa de aprendizaje menor, el entrenamiento mostró menos valores inconsistentes en las pérdidas tanto de validación como de entrenamiento. Además, los valores de precisión en entrenamiento y validación mostraron una mayor similitud. Al probar el modelo, el valor estuvo acorde al correspondiente en entrenamiento y validación, mostrando entre todos valores finales de alrededor del 75%.

<img width="567" height="455" alt="téléchargement (3)" src="https://github.com/user-attachments/assets/b17d9071-7dcf-43e2-8f75-c84beba9a628" />


En las matrices de confusión generadas por el modelo, se puede observar una mayor precisión en la clasificación de mariposas.

<img width="703" height="654" alt="cm1" src="https://github.com/user-attachments/assets/187a10bb-6091-493e-8d0d-728b35c05f94" />
<img width="661" height="612" alt="cm2" src="https://github.com/user-attachments/assets/b01b038e-8e51-4aa9-a381-c24d76fc466e" />
<img width="697" height="648" alt="cm3" src="https://github.com/user-attachments/assets/f64367c8-c8fe-4815-83f1-db21ed1026ab" />
<img width="634" height="586" alt="cm4" src="https://github.com/user-attachments/assets/c4a0f024-6f92-49f8-917a-e931e5506318" />
<img width="654" height="606" alt="cm5" src="https://github.com/user-attachments/assets/9f84d295-fb60-4ce2-9fb2-5c85a3967d6a" />

## Conclusiones

El experimento muestra resultados satisfactorios y de mejora, ya que se puede observar cómo un modelo cambia de underfitting a overfitting, para finalmente dar resultados coherentes entre los conjuntos de entrenamiento, validación y pruebas. Es complicado que el modelo llegue a un resultado completamente perfecto (100% de precisión) debido a la cantidad de clases que existen en el set de datos. Pese a esto, las mejoras presentadas a partir de las decisiones tomadas muestran resultados importantes, los cuales han sido demostrados con gráficas, mapas y métricas.

## Referencias
D. P. Mishra, T. K. Tripathy, and S. Chakraborty, CNN for Butterfly Classification, vol. 15. 2018, pp. 200–205. doi: 10.1109/icrieece44171.2018.9008419.
N. N. K. Arzar, N. Sabri, N. F. M. Johari, A. A. Shari, M. R. M. Noordin, and S. Ibrahim, Butterfly Species Identification Using Convolutional Neural Network (CNN). 2019, pp. 221–224. doi: 10.1109/i2cacis.2019.8825031.
