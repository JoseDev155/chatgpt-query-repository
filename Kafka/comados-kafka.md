# **Iniciar Zookeeper y Kafka**
Para ejecutar Zookeeper y Kafka en Windows, utiliza las herramientas de línea de comandos incluidas en Kafka:

1. **Inicia Zookeeper:**
   - Abre una ventana de **CMD** o **PowerShell** y navega al directorio `bin\windows`:
     ```bash
     cd C:\kafka_2.13-3.9.0\bin\windows
     ```
   - Ejecuta el siguiente comando para iniciar Zookeeper:
     ```bash
     zookeeper-server-start.bat ../../config/zookeeper.properties
     ```
     o
     ```bash
     .\zookeeper-server-start $env:KAFKA_HOME/config/zookeeper.properties
     ```

2. **Inicia Kafka:**
   - Abre otra ventana de **CMD** o **PowerShell** (deja Zookeeper corriendo) y navega al mismo directorio.
   - Ejecuta el siguiente comando para iniciar el servidor Kafka:
     ```bash
     kafka-server-start.bat ../../config/server.properties
     ```
     o
     ```bash
     .\kafka-server-start $env:KAFKA_HOME/config/server.properties
     ```

---

## **4. Verifica la instalación**
1. **Crea un tema (topic):**
   - Kafka utiliza temas para organizar los mensajes. En una nueva ventana de CMD, crea un tema llamado `test`:
     ```bash
     kafka-topics.bat --create --topic test --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
     ```
   - Deberías ver un mensaje confirmando que el tema `test` se creó correctamente.

2. **Lista los temas:**
   ```bash
   kafka-topics.bat --list --bootstrap-server localhost:9092
   ```

3. **Prueba productor y consumidor:**
   - **Inicia un productor (producer):**
     ```bash
     kafka-console-producer.bat --topic test --bootstrap-server localhost:9092
     ```
     Escribe algunos mensajes y presiona **Enter** después de cada uno.

   - **Inicia un consumidor (consumer):**
     Abre otra ventana de CMD y ejecuta:
     ```bash
     kafka-console-consumer.bat --topic test --from-beginning --bootstrap-server localhost:9092
     ```
     Verás los mensajes enviados desde el productor.

---

## **5. Apagar Kafka y Zookeeper**
Cuando termines de usar Kafka, apaga los procesos de forma segura:

1. Para Zookeeper:
   - Ve a la ventana donde está corriendo Zookeeper y presiona **Ctrl + C**.

2. Para Kafka:
   - Ve a la ventana donde está corriendo Kafka y presiona **Ctrl + C**.
