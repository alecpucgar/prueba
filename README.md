# Recomendación de Avatares

## Integrantes
- **Pablo Cuervo García**
- **Hugo Fraile Martínez**
- **Alejandro Puche García**

## Configuración del entorno

Para ejecutar el sistema de recomendación, es necesario contar con un entorno adecuado. Se ha utilizado un entorno basado en Python, con las siguientes dependencias principales:

```bash
pip install numpy faiss-cpu scikit-learn matplotlib
```

## Despliegue del sistema

Para desplegar los contenedores necesarios, se debe ejecutar el siguiente comando:

```bash
docker compose up
```

Una vez desplegado, se puede acceder a Jupyter Lab y a la base de datos en `localhost:8899` con el token `milvus`.

## Tareas realizadas

Se han desarrollado diversas tareas dentro del sistema de recomendación:

### Carga de datos
Ver el notebook [`jupyter_volume/1_carga_datos.ipynb`](jupyter_volume/1_carga_datos.ipynb) para revisar cómo se han cargado los embeddings de los avatares en la base de datos.

### Búsqueda por similitud
Se ha implementado una búsqueda basada en la similitud del coseno, utilizando el índice HNSW para mejorar la eficiencia. Ver el notebook [`jupyter_volume/2_busqueda_similitud.ipynb`](jupyter_volume/2_busqueda_similitud.ipynb) para más detalles.

## Pruebas de rendimiento

Se han realizado pruebas de rendimiento para evaluar la eficiencia del índice HNSW en comparación con una búsqueda exhaustiva. Los resultados muestran una reducción significativa en el tiempo de consulta. Ver el notebook [`jupyter_volume/3_pruebas_rendimiento.ipynb`](jupyter_volume/3_pruebas_rendimiento.ipynb) para más detalles.

## Conclusiones

El uso de la métrica del coseno junto con el índice HNSW ha permitido mejorar la eficiencia del sistema de recomendación de avatares, logrando búsquedas más rápidas sin comprometer la calidad de los resultados.
