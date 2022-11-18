## Aplicación

Esta aplicación utiliza algoritmo de Machine Learning para calcular la áreas
de segmentación de una imagen, dadas una anotaciones proporcionadas por el
usuario. Además de plotly y Dash para la parte gráfica se utlizaron las librerías
[scikit-image](https://scikit-image.org/) y [scikit-learn](https://scikit-learn.org)


### Cálculo de las features

En machine learning, una muestra es representada como un vector de características *features*. Los modelos de Deep learning aprenden las características directamente de los datos y son muy populares en el procesamiento de imágenes digitales. Sin embargo, estos modelos requieren una muestra de entrenamiento muy grande, es decir, un gran número de imágenes, por lo tanto su entrenamiento es muy costoso (tanto en recursos como en tiempo). En este caso se utilizaron características locales, las cuales para cada pixel representan:

- La intensidad promedio en una región pequeña alrededor de cada pixel.
- La magnitud promedio de gradientes en la misma región.
- Mediciones de textura local en la región.

Tales características son calculadas primero realizando convoluciones de la imagen con un kernel Gaussiano. Y luego realizando las mediciones de intensidad de color, gradientes o los eigenvalores de la matriz Hessiana. Todas estas operaciones están disponibles en los modulos de [`filters` module of `scikit-image`](https://scikit-image.org/docs/stable/api/skimage.filters.html) y son relativamente rápidos, ya que operan en vecindarios locales.

## Entrenamiento del modelo y predicción

Las características de los pixeles etiquetados son extraídas y luego se pasan a un clasificador [Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) de scikit-learn. Este estimador pertenece a la clase de [ensemble methods](https://scikit-learn.org/stable/modules/ensemble.html), donde las prediciones de varios estimadores base se combinan para mejorar la generalización y robustez de la prediccción. Después de que el modelo es entrenado la predicción se calcula en los pixeles que no estan etiquetados, resultando en una imagen segmentada. Es posible añadir más anotaciones (etiquetas) para mejorar la segmentación si los pixeles fueron clasificados erróneamente. El estimador se puede descargar para realizar clasificaciones en nuevas imagenes del mismo tipo.
