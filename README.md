# clustering

Práctico de Clustering para el curso [Text Mining](https://sites.google.com/view/text-mining-2019/)

## objetivo

Encontrar grupos de palabras que puedan ser usados como clases de equivalencia, al estilo de los [Brown Clusters](https://en.wikipedia.org/wiki/Brown_clustering). En mi caso conseguir clusters que se relacionen con noticias que aparecieron en el periodico durante el mes de mayo.

## detalles técnicos

Se utilizó una recopilación de noticias del Periodico durante el mes de mayo de 2004.

Se utilizaron las siguientes herramientas:
* [nltk]
* [scikit-learn]
* [gensim]
* [spacy]

### normalización
Para normalizar las palabras se dividió el texto en sentencias y para cada sentencia, se creó una lista de tokens utilizando nltk. Luego, para cada lista de tokens
* todos los tokens fueron expresados en lowercase,
* se eliminaron los tokens que tenian caracteres no alfabéticos, 
* se eliminaron las _stopwords_ del lenguaje español (palabras muy frecuentes en el lenguaje que aportan poco valor) definidas en nltk,
* y finalmente se utilizó un proceso de lematización de cada palabra (determinar el lemma de una palabra dada).

### vectorización 

Para vectorizar las palabras se probaron dos estrategias diferentes:

**Vectorización con reducción de dimensionalidad mediante umbral de frecuencia**

* Se construyó la matriz de co-ocurrencia entre palabras en un contexto dado (todas las palabras de la oracion).
* Se redujo la dimensionalidad de la matriz utilizando en las columnas sólo aquellas palabras que superaban un umbral de frecuencia dado. 
* Se obtuvieron los vectores a partir de las filas de la matriz resultante.
* Se conto la frecuencia en que cada palabra aparecia con cierto DEP_ y POS_
* Se conto la frecuenca de cada uno de los tags definidos por Spacy
* Se elimino de contexto a las palabras que solo aparecian una vez en el corpus

**Word embeddings neuronales**

* Se crearon los vectores de palabras a partir de una implementación de [word2vec](https://en.wikipedia.org/wiki/Word2vec), que aprende vectores para representar las palabras utilizando redes neuronales. 

### clustering

En ambos casos de vectorización se utilizó el algoritmo de clustering [K-means](https://en.wikipedia.org/wiki/K-means_clustering)

## resultados

En las ejecuciones el número de clusters elegido fue 100.

**Clustering sobre vectores a partir de matriz de co-ocurrencias**

Para este caso se utilizaron los siguientes parámetros:
* **Frecuencia mínima** = 10 (sólo se consideraron las columnas correspondientes a palabras que ocurrieran al menos 10 veces)
* **Tamaño de ventana** = Toda la oracion en la que aparece.
* **Tamaño de oracion** = se tomaron solo oraciones mas largas que 30 caracteres.
* **Frecuencia minima de una palabra para aparecer en contexto** = 2
