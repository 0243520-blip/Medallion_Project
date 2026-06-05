[5:40 PM, 6/5/2026] Andy Alcalá ✨: ## 1. Overview
Este proyecto implementa un pipeline de datos tipo Medallion para procesar archivos Parquet de viajes de taxis en Nueva York (2025).  
El flujo sigue la arquitectura Bronze → Silver → Gold:

- Bronze: Archivos crudos descargados, sin modificación.
- Silver: Datos procesados y limpios por cada archivo.
- Gold: Dataset final unificado listo para análisis o visualización.

El proyecto está preparado para ejecutarse localmente con Spark o escalar a AWS/EMR/S3 y es clonable.

---

## 2. Project Structure

Medallion_Project/
├── bronze/                  # Datos crudos (raw Parquet, carpeta vacía con .gitkeep)
├── silver/                  # Datos procesados y limpios (vacía con .gitkeep)
├── gold/                    # Dataset final unificado (vacía con .gitkeep)
├── scripts/                 # Spark jobs
│   ├── bronze_to_silver_job.py
│   └── silver_to_gold_job.py
└── README.md                # Documentación


---

## 3. Step 0: Clonar el repositorio
Para trabajar con este proyecto en otro equipo:
bash
git clone https://github.com/TU_USUARIO/Medallion_Project.git
cd Medallion_Project


---

## 4. Requirements
- Python 3.11 o superior
- Conda (para entornos)
- PySpark
- Pandas
- pyarrow o fastparquet
- Opcional: AWS S3 y EMR para ejecución en la nube

---

## 5. Preparación del entorno
1. Crear y activar entorno Conda:
bash
conda create -n spark_env python=3.11
conda activate spark_env

2. Instalar dependencias:
bash
pip install pyspark pandas pyarrow

3. Verificar instalación:
bash
python -c "import pyspark; import pandas; import pyarrow"


---

## 6. Subir datos crudos a Bronze
1. Descargar los archivos .parquet de tus fuentes.
2. Copiar los archivos a la carpeta relativa:

./bronze/

3. Opcional: subir a AWS S3:
bash
aws s3 cp yellow_tripdata_2025-01.parquet s3://mi-bucket/Medallion_Project/bronze/

4. Confirmar que los archivos estén en bronze/.

---

## 7. Ejecutar Pipeline
### 7.1 Bronze → Silver
bash
spark-submit scripts/bronze_to_silver_job.py

- Lee archivos de bronze usando rutas relativas.
- Limpia columnas y filas nulas.
- Guarda resultados en silver.
- Verificar salida: dir silver o ls silver.

### 7.2 Silver → Gold
bash
spark-submit scripts/silver_to_gold_job.py

- Lee todos los archivos de silver usando rutas relativas.
- Une en un único DataFrame.
- Guarda el dataset final en gold/final.parquet.
- Verificar salida: dir gold o ls gold.

---

## 8. Validación del dataset final
python
import pandas as pd
df = pd.read_parquet('./gold/final.parquet')
print(df.head())


---

## 9. Ejecución en AWS (Opcional)
- Subir carpetas bronze, silver y gold a S3.
- Spark sobre EMR o Databricks.
- Configurar IAM roles y permisos.
- Monitorear logs en CloudWatch.

Ejemplo comando Spark en EMR:
bash
spark-submit --deploy-mode cluster scripts/silver_to_gold_job.py --conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem --conf spark.hadoop.fs.s3a.access.key=TU_ACCESS_KEY --conf spark.hadoop.fs.s3a.secret.key=TU_SECRET_KEY s3://mi-bucket/Medallion_Project/silver


---

## 10. Buenas Prácticas
- Mantener los datos crudos en Bronze sin alterar.
- Versionar scripts con Git.
- Documentar cada job y transformación.
- Usar archivos Parquet para eficiencia.
- Separar entornos dev/prod.

---

## 11. Referencias
- [Medallion Architecture](https://databricks.com/blog/2020/11/23/medallion-architecture.html)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [AWS EMR](https://aws.amazon.com/emr/)
[5:52 PM, 6/5/2026] Andy Alcalá ✨: # Medallion Project: NY Taxi Data Pipeline

## 1. Overview
Este proyecto implementa un *pipeline de datos tipo Medallion* para procesar archivos Parquet de viajes de taxis en Nueva York (2025).  
El flujo sigue la arquitectura *Bronze → Silver → Gold* y está preparado para ejecutar análisis y modelado predictivo sobre los datos finales.

---

## 2. Project Structure

Medallion_Project/
├── bronze/                  # Datos crudos (raw Parquet, carpeta vacía con .gitkeep)
├── silver/                  # Datos procesados y limpios (vacía con .gitkeep)
├── gold/                    # Dataset final unificado (vacía con .gitkeep)
├── scripts/                 # Spark jobs y scripts de preprocesamiento
│   ├── bronze_to_silver_job.py
│   └── silver_to_gold_job.py
└── README.md                # Documentación


> Nota: Las carpetas bronze, silver y gold contienen .gitkeep para mantener la estructura al clonar.

---

## 3. Description of Each Layer

### Bronze Layer
- *Contenido:* Datos crudos tal como se descargan. Archivos .parquet diarios o mensuales de taxis.  
- *Qué se hace:* Solo lectura, no se modifica la información.  
- *Objetivo:* Mantener un historial confiable de los datos originales.  
- *Modelo usado:* Ninguno. Es la fuente de datos inicial.  

### Silver Layer
- *Contenido:* Archivos de Bronze transformados y limpios.  
- *Qué se hace:*  
  - Eliminación de columnas irrelevantes.  
  - Filtrado de filas nulas o incorrectas.  
  - Normalización de tipos de datos y fechas.  
  - Validación básica de consistencia (trip_distance ≥ 0, passenger_count > 0).  
- *Objetivo:* Generar datasets consistentes y limpios para análisis intermedio.  
- *Modelo usado:* Spark jobs (bronze_to_silver_job.py) para limpieza y procesamiento de datos.  

### Gold Layer
- *Contenido:* Dataset final unificado (final.parquet) con todos los datos limpios.  
- *Qué se hace:*  
  - Combina todos los archivos Silver en un único DataFrame.  
  - Se pueden hacer agregaciones finales opcionales.  
  - Dataset listo para análisis, dashboards o modelado predictivo.  
- *Objetivo:* Proveer una tabla maestra consolidada y confiable.  
- *Modelo usado:*  
  - Los datos de Gold se pueden utilizar para entrenamiento de modelos de Machine Learning, como *Logistic Regression, **XGBoost* o *Random Forest*.  
  - Aplicable para predicción de duración de viajes, demanda de taxis, o clasificación de trayectos.  

---

## 4. Step 0: Clonar el repositorio
bash
git clone https://github.com/TU_USUARIO/Medallion_Project.git
cd Medallion_Project


---

## 5. Requirements
- Python 3.11 o superior
- Conda (para entornos)
- PySpark
- Pandas
- pyarrow o fastparquet
- Opcional: AWS S3 y EMR

---

## 6. Preparación del entorno
bash
conda create -n spark_env python=3.11
conda activate spark_env
pip install pyspark pandas pyarrow
python -c "import pyspark; import pandas; import pyarrow"


---

## 7. Subir datos crudos a Bronze
1. Descargar archivos .parquet de la fuente.  
2. Copiar a la carpeta relativa ./bronze/.  
3. Opcional: subir a AWS S3:
bash
aws s3 cp yellow_tripdata_2025-01.parquet s3://mi-bucket/Medallion_Project/bronze/

4. Confirmar archivos en Bronze.  

---

## 8. Ejecutar Pipeline

### Bronze → Silver
bash
spark-submit scripts/bronze_to_silver_job.py

- Limpieza de datos, filtrado, creación de archivos Silver.  
- Verificar salida: dir silver o ls silver.  

### Silver → Gold
bash
spark-submit scripts/silver_to_gold_job.py

- Consolida todos los archivos Silver en gold/final.parquet.  
- Verificar salida: dir gold o ls gold.  

---

## 9. Validación del dataset final
python
import pandas as pd
df = pd.read_parquet('./gold/final.parquet')
print(df.head())


---

## 10. Ejecución en AWS (Opcional)
- Subir carpetas bronze, silver y gold a S3.  
- Spark sobre EMR o Databricks.  
- Configurar IAM roles y permisos.  
- Monitorear logs en CloudWatch.  

Ejemplo comando Spark en EMR:
bash
spark-submit --deploy-mode cluster scripts/silver_to_gold_job.py --conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem --conf spark.hadoop.fs.s3a.access.key=TU_ACCESS_KEY --conf spark.hadoop.fs.s3a.secret.key=TU_SECRET_KEY s3://mi-bucket/Medallion_Project/silver


---

## 11. Buenas Prácticas
- Mantener Bronze intacto.  
- Versionar scripts con Git.  
- Documentar transformaciones.  
- Usar archivos Parquet para eficiencia.  
- Separar entornos dev/prod.  

---

## 12. Referencias
- [Medallion Architecture](https://databricks.com/blog/2020/11/23/medallion-architecture.html)  
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)  
- [AWS EMR](https://aws.amazon.com/emr/)
