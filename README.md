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

-Las pruebas de rendimiento han sido realizadas en el propio jupyter notebook.

## Conclusiones

-Las conclusiones han sido realizadas en el propio jupyter notebook.
