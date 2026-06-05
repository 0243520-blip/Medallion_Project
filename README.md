# Medallion Project

Pipeline de datos basado en la arquitectura *Medallion* para procesar información desde fuentes crudas hasta datasets listos para análisis y consumo por aplicaciones.

## Arquitectura

El proyecto sigue el enfoque Medallion, donde los datos avanzan a través de diferentes capas de procesamiento:

text
Raw → Bronze → Silver → Gold


### Capas

| Capa | Descripción |
|--------|-------------|
| Raw | Datos originales sin procesar |
| Bronze | Transformaciones básicas y limpieza inicial |
| Silver | Integración y enriquecimiento de datos |
| Gold | Datos listos para consumo y análisis |


```text
medallion/
│
├── raw/
├── bronze/
├── silver/
├── gold/
│
├── jobs/
│   ├── bronze_job.py
│   ├── bronze_silver_job.py
│   ├── silver_job.py
│   └── silver_gold_job.py
│
├── crawlers/
├── config/
└── README.md


---

# Prerrequisitos

## Software Requerido

- Python 3.9+
- Java 8+
- Apache Spark 3.3+
- AWS CLI
- Docker (opcional)
- Git

## Permisos AWS

El usuario debe contar con acceso a:

- Amazon S3
- IAM
- AWS Glue
- CloudWatch

---

# Instalación

## Clonar el repositorio

bash
git clone <repository-url>
cd medallion


## Crear entorno virtual

bash
python -m venv medallion_env


### Linux / Mac

bash
source medallion_env/bin/activate


### Windows

bash
medallion_env\Scripts\activate


## Instalar dependencias

bash
pip install pyspark boto3 pandas numpy


---

# Configuración AWS

Configurar AWS CLI:

bash
aws configure


Ingresar:

- AWS Access Key
- AWS Secret Key
- Región AWS

Ejemplo:

text
us-east-1


---

# Ejecución Local

## Preparar carpetas

Crear las carpetas:

text
raw/
bronze/
silver/
gold/


Colocar los datasets de entrada dentro de raw/.

## Ejecutar pipeline

### 1. Raw → Bronze

bash
python jobs/bronze_job.py


### 2. Bronze → Silver

bash
python jobs/bronze_silver_job.py


### 3. Procesamiento Silver

bash
python jobs/silver_job.py


### 4. Silver → Gold

bash
python jobs/silver_gold_job.py


---

# Despliegue en AWS

## Buckets S3

Crear los siguientes buckets:

| Bucket |
|----------|
| medallion-raw |
| medallion-bronze |
| medallion-silver |
| medallion-gold |

## IAM Role

Asignar permisos para:

- AWSGlueServiceRole
- S3 Full Access
- CloudWatch Logs

## Crawlers

Crear:

- raw_crawler
- bronze_crawler
- silver_crawler

## Jobs de Glue

Crear los siguientes jobs:

1. bronze_job
2. bronze_silver_job
3. silver_job
4. silver_gold_job

Asociar cada Job con:

- Script correspondiente
- IAM Role configurado

---

# Orden de Ejecución

text
bronze_job
    ↓
bronze_silver_job
    ↓
silver_job
    ↓
silver_gold_job


---

# Monitoreo

Los logs de ejecución pueden consultarse en:

- AWS CloudWatch

Revisar periódicamente:

- Errores de ejecución
- Problemas de permisos
- Consumo de recursos

---

# Buenas Prácticas

- Mantener nombres consistentes para buckets y rutas.
- Realizar pruebas con subconjuntos de datos antes de ejecutar cargas completas.
- Versionar scripts mediante Git.
- Documentar cambios de configuración.
- Monitorear ejecuciones mediante CloudWatch.

---

# Tecnologías Utilizadas

- Python
- Apache Spark
- AWS S3
- AWS Glue
- AWS IAM
- AWS CloudWatch
- Boto3
- Pandas
- NumPy

---

# Autor

Equipo de desarrollo Medallion Project.
