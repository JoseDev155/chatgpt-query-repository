Para cumplir con los requisitos especificados en la oferta laboral, te propongo un programa en Python que interactúa con una base de datos relacional (puede ser Oracle o SQL Server) y también consume una API RESTful para realizar tareas de integración de datos. El código también incluye interacción con Azure Blob Storage y transferencia de datos en formato JSON.

### Requisitos:
1. **Interacción con bases de datos** (SQL Server/Oracle) usando SQL (SELECT, DELETE, UPDATE, CTEs).
2. **Consumo de una API RESTful** con transferencia de datos en formato **JSON**.
3. **Interacción con Azure Blob Storage** para almacenamiento de datos.
4. **Python básico** para manipulación de datos.

### Herramientas de Integración de Datos:
- **SQLAlchemy** o **pyodbc** (para SQL Server/Oracle).
- **Requests** (para consumir la API RESTful).
- **Azure Storage Blob SDK** (para interactuar con Azure Blob Storage).
- **Pandas** (opcional para la manipulación de datos antes de subirlos a la base de datos o Blob Storage).

### Estructura del Programa:
1. **Conexión a la base de datos** (SQL Server u Oracle).
2. **Realizar consultas SQL** como SELECT, DELETE, UPDATE, CTEs.
3. **Consumir una API RESTful** que devuelva datos en JSON.
4. **Subir los datos procesados a Azure Blob Storage**.

### Código en Python:

```python
import pyodbc
import requests
import json
from azure.storage.blob import BlobServiceClient
import pandas as pd

# 1. Conexión a la base de datos SQL Server/Oracle
def connect_to_db():
    conn_str = 'DRIVER={SQL Server};SERVER=tu_servidor;DATABASE=tu_base_de_datos;UID=usuario;PWD=contraseña'
    conn = pyodbc.connect(conn_str)
    return conn

# 2. Realizar consulta SQL (Ejemplo con SELECT y CTE)
def fetch_data_from_db():
    conn = connect_to_db()
    cursor = conn.cursor()
    
    # Ejemplo de SELECT y CTE
    query = """
    WITH CTE AS (
        SELECT id, nombre FROM empleados WHERE salario > 50000
    )
    SELECT * FROM CTE
    """
    cursor.execute(query)
    
    rows = cursor.fetchall()
    data = [{"id": row[0], "nombre": row[1]} for row in rows]
    
    cursor.close()
    conn.close()
    
    return data

# 3. Consumir una API RESTful
def consume_api():
    url = 'https://api.example.com/data'
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json()  # Obtenemos los datos en formato JSON
        return data
    else:
        print("Error al consumir la API:", response.status_code)
        return None

# 4. Subir datos a Azure Blob Storage
def upload_to_azure(data, container_name, blob_name):
    # Configura el cliente de Azure
    connect_str = 'tu_cadena_de_conexion_azure'
    blob_service_client = BlobServiceClient.from_connection_string(connect_str)
    
    # Convertimos los datos a JSON
    json_data = json.dumps(data)
    
    # Subir el blob
    blob_client = blob_service_client.get_blob_client(container=container_name, blob=blob_name)
    blob_client.upload_blob(json_data, overwrite=True)
    print(f'Datos subidos exitosamente a Azure Blob Storage: {blob_name}')

# 5. Función principal
def main():
    # Fetch data from database
    db_data = fetch_data_from_db()
    print("Datos de la base de datos:", db_data)
    
    # Consume data from API
    api_data = consume_api()
    if api_data:
        print("Datos de la API:", api_data)
    
    # Upload both datasets to Azure Blob Storage
    upload_to_azure(db_data, 'mi_contenedor', 'db_data.json')
    upload_to_azure(api_data, 'mi_contenedor', 'api_data.json')

if __name__ == "__main__":
    main()
```

### Explicación del Código:

1. **Conexión a la base de datos:**
   - Usamos `pyodbc` para conectarnos a una base de datos SQL Server u Oracle. La función `connect_to_db()` establece la conexión con la base de datos utilizando una cadena de conexión.
   
2. **Consulta SQL (SELECT, CTE):**
   - La función `fetch_data_from_db()` ejecuta una consulta SQL que usa un CTE para seleccionar empleados con un salario superior a 50,000.
   
3. **Consumir API RESTful:**
   - Usamos `requests.get()` para realizar una solicitud HTTP GET a una API. Los datos recibidos en formato JSON se procesan con `.json()` y se devuelven como un diccionario de Python.
   
4. **Subir datos a Azure Blob Storage:**
   - Usamos el SDK `azure-storage-blob` para subir los datos procesados (ya sea de la base de datos o de la API) a un contenedor de Azure Blob Storage. Los datos se convierten a formato JSON y se cargan en Azure como un blob.

### Herramientas de Integración de Datos:
1. **SQLAlchemy/pyodbc:**
   - `SQLAlchemy` puede ser utilizado si prefieres un ORM para manejar las consultas y relaciones entre tablas de manera más abstracta.
   - `pyodbc` es más adecuado si prefieres realizar consultas SQL directamente.
   
2. **Requests (API RESTful):**
   - `requests` es una librería simple y poderosa para interactuar con APIs RESTful. En este caso, la utilizamos para consumir datos en formato JSON.
   
3. **Azure SDK:**
   - El SDK de Azure Blob Storage nos permite interactuar con los contenedores y blobs en Azure. Es fundamental para subir y gestionar grandes volúmenes de datos.

4. **Pandas (opcional):**
   - Puedes usar **pandas** para manipular datos antes de almacenarlos en la base de datos o subirlos a Blob Storage. Pandas facilita la carga de datos en un formato tabular y puede convertir fácilmente DataFrames a JSON, CSV o Excel.

### Resumen:

Este programa cubre varias habilidades mencionadas en los requisitos:
- Interacción con bases de datos relacionales usando SQL y consultas avanzadas.
- Consumo de APIs RESTful y manipulación de datos en formato JSON.
- Integración con Azure Blob Storage para almacenamiento de datos.
- Uso de herramientas comunes como `pyodbc`, `requests` y el SDK de Azure.

Este ejemplo puede adaptarse fácilmente a otros sistemas de bases de datos o necesidades de API, y es un punto de partida para crear soluciones más complejas.