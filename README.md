Aquí tienes la plantilla lista en formato Markdown para que la copies y la pegues directamente en el archivo README.md (o README.txt) de tu repositorio de GitHub.
Incluye la aclaración sobre la máquina virtual en un recuadro destacado para el tutor o evaluador, así como toda la documentación técnica del laboratorio:
# Análisis de Datos en Tiempo Real con Apache Kafka y Spark Streaming
Curso: Big Data  
Universidad: Universidad Nacional Abierta y a Distancia (UNAD)  
Actividad: Anexo 3 - Procesamiento de Datos en Tiempo Real  
## ⚠️ Nota Aclaratoria sobre la Entrega y la Máquina Virtual (VM)
> Aviso para el Evaluador:  
> La máquina virtual en formato comprimido (.ova o .vmdk) **no se adjunta en este repositorio de GitHub** debido a las siguientes razones técnicas y estándares de la industria:
> 1. Restricción de tamaño de archivos en GitHub: Los archivos de máquinas virtuales pesan varios Gigabytes, lo que excede el límite estricto de 100 MB por archivo impuesto por la plataforma GitHub.
> 2. Buenas Prácticas de Control de Versiones: Los repositorios de código fuente están diseñados exclusivamente para alojar scripts, archivos de configuración, código fuente y documentación, no imágenes de sistemas operativos completos.
> Todo el procedimiento realizado en el entorno virtualizado (Ubuntu 22.04 LTS en VirtualBox), la arquitectura implementada, los scripts en Python y las evidencias fotográficas de ejecución se encuentran completamente detallados en este repositorio.
## 📄 Descripción del Proyecto
Este proyecto implementa una arquitectura end-to-end de análisis de datos en streaming en tiempo real:

- Productor (Kafka Producer): Script en Python (kafka_producer.py) que simula la generación periódica de lectura de sensores de IoT (ID de sensor, temperatura, humedad y marca de tiempo) enviándolos en formato JSON al tópico sensor_data de Apache Kafka.
- Consumidor (Spark Streaming Consumer): Script en PySpark (spark_streaming_consumer.py) que se conecta a Kafka en tiempo real, deserializa los mensajes y calcula métricas agregadas (promedio de temperatura y humedad) utilizando ventanas de tiempo de 1 minuto.
## Estructura del Repositorio
├── kafka_producer.py          # Script emisor de datos de sensores hacia Kafka
├── spark_streaming_consumer.py # Script procesador de streaming con Spark SQL
├── evidencias/                # Capturas de pantalla de la ejecución y Spark UI
│   ├── 01_ejecucion_productor.png
│   ├── 02_ejecucion_consumidor_batches.png
│   └── 03_spark_ui_puerto_4040.png
└── README.md                  # Documentación completa del laboratorio
Prerrequisitos e Instalación
Entorno configurado sobre máquina virtual Ubuntu Linux (vboxuser):
Instalación del cliente de Python para Kafka:
Bash
pip install kafka-python

Descarga y despliegue de Apache Kafka (v3.6.2):

Bash
wget [https://archive.apache.org/dist/kafka/3.6.2/kafka_2.13-3.6.2.tgz](https://archive.apache.org/dist/kafka/3.6.2/kafka_2.13-3.6.2.tgz)
tar -xzf kafka_2.13-3.6.2.tgz
sudo mv kafka_2.13-3.6.2 /opt/Kafka

Guía de Ejecución Paso a Paso
Paso 1: Iniciar Servicios (ZooKeeper y Kafka Server)
Ejecutar en la consola SSH/PuTTY:

Bash
# Iniciar ZooKeeper en segundo plano
sudo /opt/Kafka/bin/zookeeper-server-start.sh /opt/Kafka/config/zookeeper.properties &

# Iniciar el servidor de Apache Kafka en segundo plano
sudo /opt/Kafka/bin/kafka-server-start.sh /opt/Kafka/config/server.properties &

(Presionar Enter cuando finalice la carga de los logs en la terminal).

Paso 2: Crear el Tópico en Kafka
Bash
/opt/Kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic sensor_data

Paso 3: Iniciar el Productor de Datos (Terminal 1)

Bash
python3 kafka_producer.py

Paso 4: Iniciar el Consumidor de Spark Streaming (Terminal 2)
Abrir una segunda ventana en PuTTY, iniciar sesión y ejecutar:

Bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.3 spark_streaming_consumer.py

Evidencias de Ejecución y Monitoreo
Terminal del Productor: Transmisión continua de eventos simulados (Sent: {'sensor_id': ..., 'temperature': ..., 'humidity': ...}).
Terminal del Consumidor: Salida de Spark mostrando el resumen por micro-lotes (batches) agrupados por ventana de tiempo de 1 minuto.
Consola Web Spark UI: Accesible desde el navegador del equipo anfitrión ingresando a http://<IP_MAQUINA_VIRTUAL>:4040 (ejemplo: http://192.168.1.7:4040), donde se evidencia el estado de los Jobs, Stages y Executors activos.
(Las capturas correspondientes a cada punto se encuentran en la carpeta /evidencias de este repositorio).
