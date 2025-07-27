# 🚀 Proyecto: Pipeline ETL con Apache Airflow

**Autor:** Jeyner Suarez  
**Fecha:** Julio 2025  
**Email:** jeynersyarez@gmail.com  

---

## 🎯 Objetivo

Diseñar e implementar un pipeline de procesamiento de datos desde un archivo CSV utilizando Apache Airflow. El pipeline debe automatizar las siguientes tareas:

- Ingesta de datos.
- Limpieza y normalización.
- Transformaciones y agregaciones.
- Carga en base de datos (PostgreSQL).
- Orquestación con Airflow.
- Monitoreo básico.

---

## ⚙️ Tecnologías Utilizadas

- Docker y Docker Compose  
- Apache Airflow 2.8.1  
- Python 3.9  
- PostgreSQL 13  
- Pandas

---

## 📁 Estructura del Proyecto## 📁 Estructura del Proyecto

- data_pipeline/
- │
- ├── dags/
- │ └── dag_pipeline.py
- ├── data/
- │ ├── raw_data.csv
- │ └── cleaned_data.csv # generado automáticamente
- ├── scripts/
- ├── Dockerfile
- ├── docker-compose.yml
- └── README.md

---

## 🔄 Flujo del Pipeline

El DAG `pipeline_datos_csv` está compuesto por las siguientes tareas:

1. **Ingesta de Datos**  
   Se carga el archivo `raw_data.csv` desde la carpeta `/opt/airflow/data`.

2. **Limpieza y Normalización**  
   - Se eliminan duplicados.
   - Se eliminan valores nulos.
   - Se convierten los campos de fecha al formato `datetime`.

3. **Agregación de Datos**  
   - Se calcula el monto total por cliente (`SUM(monto)`).
   - El resultado se guarda en `aggregated_data.csv`.

4. **Carga a PostgreSQL**  
   - Se crea la tabla `ventas_agrupadas` (si no existe).
   - Se insertan los datos agregados por cliente.

---

## 📊 Datos de Ejemplo

**Contenido de `raw_data.csv`:**

| id | fecha       | cliente   | producto   | monto | moneda |
|----|-------------|-----------|------------|-------|--------|
| 1  | 2024-06-01  | Cliente A | Producto X | 1200  | USD    |
| 2  | 2024-06-01  | Cliente B | Producto Y | 500   | COP    |
| 3  | 2024-06-02  | Cliente C | Producto x | 700   | USD    |
| 4  | 2024-06-02  | Cliente A | Producto z | 900   | PEN    |


---

## 📈 Métricas del Pipeline

- ✅ **Duplicados eliminados**: 0  
- ✅ **Registros eliminados por nulos**: 0  
- ✅ **Registros finales después de limpieza**: 4  
- ✅ **Clientes únicos en agregación**: 3  
- ✅ **Filas cargadas a PostgreSQL**: 3

---

## 🔒 Sugerencias de Mejora

- Implementar logs persistentes y métricas con Prometheus o Grafana.
- Agregar validaciones de esquema con `great_expectations` o `pydantic`.
- Añadir manejo de errores con `try-except` en tareas críticas.
- Usar particiones por fecha en la tabla de destino.
- Añadir soporte para lectura de archivos JSON o Parquet.
- Integrar notificaciones por email o Slack para fallos.

---

## 🧪 Cómo Ejecutar el Proyecto

```bash
# 1. Apagar contenedores previos si están activos
docker-compose down --volumes

# 2. Levantar entorno y construir contenedores
docker-compose up --build -d

# 3. Inicializar base de datos de Airflow
docker-compose run --rm airflow airflow db init

# 4. Crear usuario administrador
docker-compose run --rm airflow airflow users create \
  --username admin --firstname Admin --lastname User \
  --role Admin --email admin@example.com --password admin

# 5. Acceder a la interfaz en http://localhost:8080


