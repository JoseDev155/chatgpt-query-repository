Aquí tienes los pasos para configurar **Apache Kafka 2.13-3.9.0** en **Windows 11** después de descargar y descomprimir los archivos:

---

## **1. Requisitos previos**
Antes de configurar Kafka, asegúrate de lo siguiente:

1. **Java instalado:**
   - Instala **Java 11** o **Java 17**.
   - Configura la variable de entorno `JAVA_HOME` apuntando al directorio de instalación del JDK.

   **Verifica la instalación de Java:**
   ```bash
   java -version
   ```

2. **Zookeeper incluido en Kafka:**
   Kafka depende de **Zookeeper** para coordinar los nodos del clúster. Las versiones recientes de Kafka ya incluyen un servidor Zookeeper básico.

---

## **2. Configuración inicial**
1. **Navega al directorio de Kafka:**
   - Ve a la carpeta donde descomprimiste Kafka, por ejemplo:  
     ```
     C:\kafka_2.13-3.9.0
     ```

2. **Crea carpetas para datos:**
   Kafka y Zookeeper necesitan carpetas para almacenar datos. Crea estas carpetas en tu sistema:

   ```bash
   mkdir C:\kafka\data\zookeeper
   mkdir C:\kafka\data\kafka
   ```

3. **Edita los archivos de configuración:**
   - Navega al directorio `config` dentro de Kafka:
     ```
     C:\kafka_2.13-3.9.0\config
     ```

   ### **a) Configuración de Zookeeper (`zookeeper.properties`)**
   - Abre el archivo `zookeeper.properties` y encuentra la línea:
     ```
     dataDir=/tmp/zookeeper
     ```
   - Cámbiala para que apunte a la carpeta que creaste:
     ```
     dataDir=C:/kafka/data/zookeeper
     ```

   ### **b) Configuración de Kafka (`server.properties`)**
   - Abre el archivo `server.properties` y encuentra la línea:
     ```
     log.dirs=/tmp/kafka-logs
     ```
   - Cámbiala para que apunte a la carpeta que creaste:
     ```
     log.dirs=C:/kafka/data/kafka
     ```
   - Opcional: Cambia el puerto predeterminado si es necesario (por defecto es `9092`):
     ```
     listeners=PLAINTEXT://:9092
     ```

---

## **3. Iniciar Zookeeper y Kafka**
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

2. **Inicia Kafka:**
   - Abre otra ventana de **CMD** o **PowerShell** (deja Zookeeper corriendo) y navega al mismo directorio.
   - Ejecuta el siguiente comando para iniciar el servidor Kafka:
     ```bash
     kafka-server-start.bat ../../config/server.properties
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

---

## **Consejos adicionales**
- **Logs:** Si Kafka no arranca o ves errores, revisa los archivos de logs en `logs/`.
- **Persistencia:** Los datos generados se almacenarán en las carpetas configuradas (`C:/kafka/data`).
- **Producción:** Para entornos de producción, considera usar Kafka con Docker o en Linux.

¿Te ayudo con alguna parte del proceso? 😊