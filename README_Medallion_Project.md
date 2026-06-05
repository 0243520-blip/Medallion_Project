# Medallion Project Pipeline 🚖✨

## Descripción
Este proyecto implementa un **pipeline de datos Medallion** usando **Apache Spark** y archivos Parquet para procesar datos de tráfico de taxis.  

Capas del pipeline:  
- **Bronze**: datos crudos  
- **Silver**: datos limpios y transformados  
- **Gold**: dataset final consolidado  

---

## Estructura del proyecto
```
Medallion_Project/
├── bronze/         # Datos crudos
├── silver/         # Datos transformados
├── gold/           # Dataset final
├── scripts/        # Jobs de Spark
│   ├── bronze_to_silver_job.py
│   ├── silver_to_gold_job.py
│   └── full_pipeline_job.py
└── README.md       # Documentación
```

---

## Requisitos
- Python 3.11  
- Anaconda  
- Apache Spark  
- Pandas  
- pyarrow o fastparquet  
- Java 17  

---

## Uso
1. Activar entorno y moverse al proyecto:
```bash
conda activate spark_env
cd C:\Users\Prueba\Desktop\Medallion_Project
```
2. Ejecutar jobs:
```bash
spark-submit scripts/bronze_to_silver_job.py
spark-submit scripts/silver_to_gold_job.py
```
3. Validar resultado:
```bash
python -c "import pandas as pd; df = pd.read_parquet('gold/final.parquet'); print(df.head())"
```

---

## Referencias
- [JPLDhabla GitHub](https://github.com/amguzmans/JPLDhabla)  
- [tldr GitHub](https://github.com/tldr-pages/tldr)  
- [pyprobml GitHub](https://github.com/probml/pyprobml)
