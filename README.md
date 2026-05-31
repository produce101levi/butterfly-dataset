# Clasificación con Machine Learning de Diferentes Tipos de Mariposas

## Abstract

## Introducción

## Metodología
### Modelo base
El experimento se inició con un modelo de inteligencia artificial con una red neuronal convolucional (CNN por sus siglas en inglés) que contaba con los siguientes parámetros

[parámetros]

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
Este modelo tiene una arquitectura muy simple, contando con únicamente 3 layers.

## Conclusiones

## Referencias
