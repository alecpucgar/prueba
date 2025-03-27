# Recomendación de Avatares

## Integrantes
- **Pablo Cuervo García**
- **Hugo Fraile Martínez**
- **Alejandro Puche García**

## Despliegue del sistema

Para desplegar los contenedores necesarios, se debe ejecutar el siguiente comando:

**docker compose up -d**

Este comando ejecuta un archivo docker compose el cual despliega tanto la base de datos de reddis(puerto 8001) como el propio jupyter notebook(puerto 8888) donde se trabajará la práctica principalmente.

Una vez desplegado, se puede acceder a Jupyter Lab y a la base de datos .

## Tareas realizadas

-En primer lugar, se introdujeron las imágenes que habían sido previamente descargadas del moodle de la asignatura en la carpeta **avatar_portraits**, tras esto se utilizó la función proprocionada para convertir estas imágenes en embeddings y almacenarlos para su posterior uso. Finalmente, se realizó la conexión a la base de datos de **Reddis** necesaria para el desarrollo de la práctica.

-En segundo lugar, mediante al comando **execute_command** se creará un primer índice llamado **clip_index** utilizando el índice **flat**, en el que se introducirán 6 parámetros especificando que sea de tipo **float 32** tenga dimensión **512** y se use como métricas de distancia el **coseno**, ya que el enunciado lo requiere. Tras esto, se relizará un código que mediante a un bucle for ira iterando embedding tras embedding almacenando cada uno de ellos en un conjunto junto al nombre de la imagen, se utilizará el método previamente definido **to_blot** para realizar el paso a bytes de los embedding.

-En tercer lugar se completará la función **search_by_text_top_3**, la cual realiza una búsqueda de similitud basada en embeddings para recuperar imágenes similares a un texto dado, esta tomará un texto de entrada el cuál convierte en un vector , tras esto mediante una **Query** de consulta **KNN (K-Nearest Neighbors)** buscará las tres imágenes mas cercanas y devolverá la imagen y la similitud en cuestión con el texto especificado. Finalemnete se ejecutará la búsqueda en **Reddis** pasando dicho embedding al formato convertido y después utilizará la función **draw_images** para mostrar las fotos.

-Finalmente se programará un último método que se encargará de buscar las imágenes mas similares a una dada, esta función es muy similar a la anterior, diferenciándose únicamente en que en vez de crear un embedding de un texto se creará el de una imagen, tras esto el resto del proceso se realizará de manera homogénea.

## Pruebas de rendimiento

### Búsqueda por Imágenes Similares

#### Algoritmo HNSW
- **clip_index_hnsw_l2**: 232 μs ± 4.64 μs per loop
- **clip_index_hnsw_IP**: 237 μs ± 8.88 μs per loop
- **clip_index_hnsw_cosine**: 216 μs ± 5.86 μs per loop

#### Algoritmo FLAT
- **clip_index_flat_l2**: 861 μs ± 757 μs per loop
- **clip_index_flat_ip**: 284 μs ± 1.69 μs per loop
- **clip_index_flat_cosine**: 291 μs ± 12.5 μs per loop

#### **Análisis de Resultados: Imágenes Similares**
1. **Eficiencia Temporal**
   En la búsqueda por imágenes similares, **HNSW demuestra tiempos de ejecución menores** (entre 216 μs y 237 μs) en comparación con FLAT (284 μs a 291 μs en índices IP y cosine).

2. **Precisión vs. Rendimiento**
   - **FLAT**, al realizar una búsqueda exhaustiva por fuerza bruta, garantiza el **mejor resultado** en precisión, siendo ideal cuando la exactitud es fundamental.
   - **HNSW**, al ser una búsqueda aproximada, ofrece tiempos más rápidos pero puede introducir pequeños errores.

3. **Influencia del Tamaño de la Base de Datos**
   La base de datos utilizada en este ejemplo es pequeña, por lo que las diferencias de tiempo no son drásticas. Con bases de datos más grandes, **HNSW** sería más eficiente en términos de velocidad, aunque **FLAT** seguiría ofreciendo resultados óptimos en precisión.

---

### Búsqueda por Descripción

#### Algoritmo HNSW
- **clip_index_hnsw_l2**: 218 μs ± 7.04 μs per loop
- **clip_index_hnsw_IP**: 225 μs ± 4.11 μs per loop
- **clip_index_hnsw_cosine**: 213 μs ± 4.4 μs per loop

#### Algoritmo FLAT
- **clip_index_flat_l2**: 288 μs ± 2.92 μs per loop
- **clip_index_flat_ip**: 287 μs ± 3.84 μs per loop
- **clip_index_flat_cosine**: 282 μs ± 3.34 μs per loop

#### **Análisis de Resultados: Búsqueda por Descripción**
1. **Eficiencia Temporal**
   En la búsqueda por descripción, **HNSW sigue siendo ligeramente más rápido** (213 μs a 225 μs) en comparación con FLAT (282 μs a 288 μs), aunque la diferencia es menor que en la búsqueda por imágenes.

2. **Precisión vs. Rendimiento**
   - **FLAT** asegura la máxima precisión al ejecutar una búsqueda exhaustiva, lo que es ventajoso cuando la precisión es crítica.
   - **HNSW** permite una mayor eficiencia temporal, lo que puede ser preferible en entornos donde la velocidad es esencial y se tolera una ligera aproximación en los resultados.

3. **Influencia del Tamaño de la Base de Datos**
   Al igual que en la búsqueda por imágenes, la base de datos pequeña utilizada en este experimento minimiza la diferencia en tiempos. Con bases de datos de mayor tamaño, **HNSW** se destacaría en velocidad, aunque **FLAT** seguiría siendo el método que garantiza los mejores resultados en precisión.


## **Conclusión**

- **FLAT ofrece siempre el mejor resultado en términos de precisión** debido a su enfoque de búsqueda por fuerza bruta, lo que lo hace ideal para aplicaciones donde la exactitud es primordial.
- En este ejemplo, la diferencia de tiempos no es tan significativa porque la base de datos es pequeña. Sin embargo, **HNSW se muestra más eficiente en términos de velocidad**, especialmente en la búsqueda por imágenes similares, lo que lo convierte en la opción preferida en entornos con bases de datos extensas.
- Es importante notar que, aunque **HNSW** ofrece tiempos de ejecución menores en bases de datos grandes, **no garantiza siempre el mejor resultado en precisión**. La elección del algoritmo dependerá de las prioridades de la aplicación:
  - **Precisión absoluta:** FLAT es el más indicado.
  - **Velocidad y eficiencia en grandes volúmenes de datos:** HNSW resulta más eficiente, siempre que se acepte una leve aproximación en los resultados.
