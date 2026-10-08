Requisitos previos. Necesitas Python 3.10 o superior, Java 11, Apache Spark 3.5.0, MongoDB 7.0, y las dependencias declaradas en el archivo requirements.txt. También necesitas una cuenta de Kaggle con su API key, y opcionalmente una cuenta de ngrok para exponer la API públicamente.

Paso 1. Clonar el repositorio. Abre una terminal y ejecuta git clone https://github.com/TU_USUARIO/bigdata-geoespacial.git, luego entra al directorio con cd bigdata-geoespacial.

Paso 2. Instalar dependencias. Ejecuta pip install -r requirements.txt. Esto instalará pymongo, dask, pyspark, flask, flask-cors, pyngrok, pytest, requests, kaggle, geohash2, shapely, pandas y numpy.

Paso 3. Configurar credenciales. Las credenciales nunca se suben al repositorio. Define las variables de entorno KAGGLE_USERNAME y KAGGLE_KEY con tus credenciales de Kaggle. Si usas Google Colab, configúralas como Secretos con esos mismos nombres. Si vas a exponer la API con ngrok, configura también NGROK_AUTH_TOKEN.

Paso 4. Levantar MongoDB. Crea los directorios de datos y logs y lanza el servidor en modo demonio sobre el puerto 27017 con bind a la dirección local. El comando es mongod con las banderas dbpath, logpath, port, bind_ip y fork. Al terminar, verifica la conexión con un cliente de MongoDB.

Paso 5. Abrir el notebook. Carga el archivo notebooks/Trabajo_BigData_Geoespacial.ipynb en Jupyter o Google Colab.

Paso 6. Ejecutar el pipeline. Ejecuta las celdas del notebook en orden. El notebook realiza la descarga del dataset desde Kaggle, la limpieza con Dask, la carga de los registros limpios a MongoDB como documentos GeoJSON con índice 2dsphere, el procesamiento con Spark para calcular las agregaciones espaciales y temporales, y la escritura de los resultados en colecciones nuevas de MongoDB.

Paso 7. Levantar la API. Ejecuta python src/api.py. La API queda disponible en http://127.0.0.1:5000. Si configuraste ngrok, se mostrará también una URL pública.

Paso 8. Ejecutar pruebas. En otra terminal, ejecuta pytest tests/ -v. Las siete pruebas validan el endpoint de salud, las consultas geoespaciales con parámetros válidos e inválidos, y las consultas a los resultados de Spark.

Paso 9. Verificar la aplicación. Abre en el navegador http://127.0.0.1:5000/health para confirmar que la API responde. También puedes probar los endpoints /near, /within y /spark-results con parámetros de ejemplo.

Paso 10. Ejecutar el pipeline de integración continua. Ejecuta el script del pipeline que realiza en orden el checkout, la instalación, la validación de sintaxis, las pruebas unitarias, el levantamiento de servicios, las pruebas de humo y el despliegue condicionado. Si alguna prueba falla, el despliegue se bloquea automáticamente.
