📦 FleetLogix Data Engineering Project

Modernización completa del ecosistema de datos para una empresa de logística con flota de 200 vehículos

📘 Introducción

FleetLogix es una empresa de transporte y logística que opera entregas de última milla en 5 ciudades. Ante la necesidad de modernizar sus sistemas legacy y planillas, el proyecto consiste en construir una solución de datos integral que abarque:

Modelado relacional en PostgreSQL

Generación masiva de datos sintéticos realistas

Validación de integridad referencial

Consultas avanzadas para resolver problemas operativos

Migración y procesamiento en Snowflake

Arquitectura cloud serverless en AWS

Pipelines automáticos de ingesta y análisis

Este proyecto fue desarrollado en el marco del Módulo 2 – Data Science.

🎯 Objetivos del Proyecto

Dominar SQL (CTEs, Window Functions, optimización de queries)

Generar datos sintéticos masivos con integridad y realismo

Transformar un modelo OLTP en un Data Warehouse dimensional

Implementar arquitectura cloud con AWS (RDS, S3, Lambda, API Gateway, DynamoDB)

Integrar bases NoSQL para datos no estructurados (MongoDB/DynamoDB)

Desarrollar pipelines ETL en Python

Aplicar buenas prácticas de arquitectura de datos

🗄️ Modelo Relacional en PostgreSQL

El modelo proporcionado incluye 6 tablas principales:

▫ Tablas Dimensión

vehicles

drivers

routes

▫ Tablas de Hechos

trips

deliveries

maintenance

Se documentaron claves primarias, foráneas, constraints e índices.
Posteriormente se creó el esquema completo en PostgreSQL con scripts de creación, índices y comentarios, y una tabla adicional logs_ingesta para registrar cada carga de datos.

🧪 Generación de Datos Sintéticos (Python)

Para poblar la base se utilizó:

psycopg2 → conexión a PostgreSQL

Faker → generación de datos realistas

random y numpy → control vía seed (RANDOM_SEED = 42)

Funciones auxiliares para:

distribuciones horarias realistas de viajes

validación de consistencia temporal

simulación de rutas, vehículos, conductores, cargas y estados

creación de logs automáticos

Se poblaron todas las tablas manteniendo integridad referencial.

🔍 Verificación y Validación de Datos

Se ejecutaron consultas para validar:

Cantidad de registros por tabla

Primeros registros de cada entidad

Conteo por estado (vehículos, viajes, entregas)

Viajes por tipo de vehículo

Entregas por conductor

Consistencia temporal (arrivals > departures)

Fechas de ingesta en logs

📊 Avance 2 – Análisis de Queries Operativas

Se ejecutaron 12 queries orientadas a problemas reales de negocio:

🔹 Queries básicas

Composición de la flota

Conductores con licencia próxima a vencer

Viajes por estado

🔹 Queries intermedias

Demanda por ciudad

Conductores activos con más viajes completados

Promedio de entregas por conductor

🔹 Queries complejas

Costo de mantenimiento por kilómetro

Entregas por día y horario de semana

Para cada una se analizó el plan de ejecución, se justificó la necesidad operativa y se crearon índices optimizadores comparando tiempos con y sin índice.

❄️ Avance 3 – Migración a Snowflake

Se creó un entorno OLTP equivalente en Snowflake:

✔ Proceso realizado

Conexión desde Python

Carga de múltiples DataFrames con datos exportados de PostgreSQL

Transformación y limpieza según estructura del warehouse

Creación de vistas

Verificación de cargas completas

Script de automatización de ingestas diarias

☁️ Avance 4 – Arquitectura Cloud en AWS

Se diseñó e implementó una arquitectura para ingesta y análisis en tiempo real:

🏗️ Arquitectura Serverless AWS
🔹 Capa 1 – Entrada

API Gateway recibe eventos desde la app móvil:

Estado de entregas

GPS del conductor

Inicio/fin de viajes

🔹 Capa 2 – Procesamiento

Funciones AWS Lambda:

Verificar entrega (API Gateway → Lambda → DynamoDB)

Calcular ETA cada 5 minutos (EventBridge)

Detectar desvíos de ruta (Kinesis → Lambda → SNS)

🔹 Capa 3 – Almacenamiento

DynamoDB: estados, tracking, alertas

S3: datos históricos, backups, logs

RDS PostgreSQL: sistema transaccional base

🔹 Capa 4 – Notificaciones

SNS para alertas inmediatas

EventBridge para automatizaciones programadas

🧰 Script de AWS Automation (aws_setup.py)

Crea automáticamente:

Recurso	Servicio	Función
RDS PostgreSQL	Amazon RDS	Base relacional transaccional
Bucket S3	Amazon S3	Almacenamiento raw/processed/backups
Tablas DynamoDB	AWS DynamoDB	Tracking, alertas, estados
IAM Role para Lambda	AWS IAM	Permisos para acceso entre servicios
Snapshot inicial	RDS	Backups automáticos
Script migrate_to_rds.sh	Shell	Migración de BD local a RDS

Incluye funciones como:

crear_rds_postgresql()

crear_s3_bucket()

crear_dynamodb_tables()

crear_iam_role_lambda()

generación de estructura y políticas (Lifecycle, Tags, etc.)

🧠 Conclusión del Proyecto

El proyecto FleetLogix integra:

✔ Modelado relacional
✔ Generación masiva de datos sintéticos
✔ Queries avanzadas para resolver problemas reales
✔ Migración y ejecución en Snowflake
✔ Arquitectura cloud escalable y serverless en AWS
✔ Pipelines automáticos y buenas prácticas de ingeniería de datos

Se logró transformar un modelo transaccional básico en una solución moderna orientada a análisis en tiempo real y toma de decisiones basada en datos.
