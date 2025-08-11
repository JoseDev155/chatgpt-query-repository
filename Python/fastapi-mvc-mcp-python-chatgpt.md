# Prompt



# Respuesta

Perfecto, Eduardo. Vamos por partes. Primero crearé el ejemplo completo siguiendo la arquitectura MVC con FastAPI, MySQL, y el patrón Repository. Luego te explicaré cómo migrarlo a MCP (Model-Controller-Presenter) y te daré el código adaptado.

---

## ✅ Parte 1: Ejemplo completo con **MVC + FastAPI + Repository + MySQL**

---

### 📁 Estructura del proyecto

```
project/
│
├── app/
│   ├── controllers/
│   │   ├── user_controller.py
│   │   └── task_controller.py
│   │
│   ├── models/
│   │   ├── user_model.py
│   │   └── task_model.py
│   │
│   ├── repositories/
│   │   ├── user_repository.py
│   │   └── task_repository.py
│   │
│   ├── routes/
│   │   ├── user_routes.py
│   │   └── task_routes.py
│   │
│   ├── database.py
│   └── main.py
│
└── requirements.txt
```

---

### 📦 `requirements.txt`

```txt
fastapi
uvicorn
mysql-connector-python
pydantic
```

---

### ⚙️ `app/database.py`

```python
import mysql.connector

def get_connection():
    return mysql.connector.connect(
        host="localhost",
        user="root",
        password="tu_contraseña",
        database="mi_basededatos"
    )
```

---

### 🧩 Modelos (`models/`)

#### `user_model.py`

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int | None = None
    name: str
    email: str
```

#### `task_model.py`

```python
from pydantic import BaseModel

class Task(BaseModel):
    id: int | None = None
    title: str
    description: str
    user_id: int
```

---

### 📁 Repositorios (`repositories/`)

#### `user_repository.py`

```python
from app.database import get_connection
from app.models.user_model import User

class UserRepository:
    def create(self, user: User):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("INSERT INTO users (name, email) VALUES (%s, %s)", (user.name, user.email))
        conn.commit()
        conn.close()

    def get_by_id(self, user_id: int) -> User | None:
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))
        row = cursor.fetchone()
        conn.close()
        return User(id=row[0], name=row[1], email=row[2]) if row else None

    def update(self, user: User):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("UPDATE users SET name = %s, email = %s WHERE id = %s", (user.name, user.email, user.id))
        conn.commit()
        conn.close()

    def delete(self, user_id: int):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("DELETE FROM users WHERE id = %s", (user_id,))
        conn.commit()
        conn.close()
```

#### `task_repository.py`

```python
from app.database import get_connection
from app.models.task_model import Task

class TaskRepository:
    def create(self, task: Task):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute(
            "INSERT INTO tasks (title, description, user_id) VALUES (%s, %s, %s)",
            (task.title, task.description, task.user_id)
        )
        conn.commit()
        conn.close()

    def get_by_id(self, task_id: int) -> Task | None:
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM tasks WHERE id = %s", (task_id,))
        row = cursor.fetchone()
        conn.close()
        return Task(id=row[0], title=row[1], description=row[2], user_id=row[3]) if row else None

    def update(self, task: Task):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute(
            "UPDATE tasks SET title = %s, description = %s, user_id = %s WHERE id = %s",
            (task.title, task.description, task.user_id, task.id)
        )
        conn.commit()
        conn.close()

    def delete(self, task_id: int):
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("DELETE FROM tasks WHERE id = %s", (task_id,))
        conn.commit()
        conn.close()
```

---

### 📂 Controladores (`controllers/`)

#### `user_controller.py`

```python
from app.repositories.user_repository import UserRepository
from app.models.user_model import User

user_repo = UserRepository()

def create_user(user: User):
    user_repo.create(user)
    return {"message": "Usuario creado"}

def get_user(user_id: int):
    user = user_repo.get_by_id(user_id)
    return user if user else {"error": "No encontrado"}

def update_user(user: User):
    user_repo.update(user)
    return {"message": "Usuario actualizado"}

def delete_user(user_id: int):
    user_repo.delete(user_id)
    return {"message": "Usuario eliminado"}
```

#### `task_controller.py`

```python
from app.repositories.task_repository import TaskRepository
from app.models.task_model import Task

task_repo = TaskRepository()

def create_task(task: Task):
    task_repo.create(task)
    return {"message": "Tarea creada"}

def get_task(task_id: int):
    task = task_repo.get_by_id(task_id)
    return task if task else {"error": "No encontrada"}

def update_task(task: Task):
    task_repo.update(task)
    return {"message": "Tarea actualizada"}

def delete_task(task_id: int):
    task_repo.delete(task_id)
    return {"message": "Tarea eliminada"}
```

---

### 🌐 Rutas (`routes/`)

#### `user_routes.py`

```python
from fastapi import APIRouter
from app.controllers import user_controller
from app.models.user_model import User

router = APIRouter()

@router.post("/")
def create_user(user: User):
    return user_controller.create_user(user)

@router.get("/{user_id}")
def get_user(user_id: int):
    return user_controller.get_user(user_id)

@router.put("/")
def update_user(user: User):
    return user_controller.update_user(user)

@router.delete("/{user_id}")
def delete_user(user_id: int):
    return user_controller.delete_user(user_id)
```

#### `task_routes.py`

```python
from fastapi import APIRouter
from app.controllers import task_controller
from app.models.task_model import Task

router = APIRouter()

@router.post("/")
def create_task(task: Task):
    return task_controller.create_task(task)

@router.get("/{task_id}")
def get_task(task_id: int):
    return task_controller.get_task(task_id)

@router.put("/")
def update_task(task: Task):
    return task_controller.update_task(task)

@router.delete("/{task_id}")
def delete_task(task_id: int):
    return task_controller.delete_task(task_id)
```

---

### 🧵 `main.py`

```python
from fastapi import FastAPI
from app.routes import user_routes, task_routes

app = FastAPI()

app.include_router(user_routes.router, prefix="/users", tags=["Users"])
app.include_router(task_routes.router, prefix="/tasks", tags=["Tasks"])
```

---

## 🔄 Parte 2: Migrar de **MVC a MCP (Model-Controller-Presenter)**

### 🎯 ¿Qué cambia?

* **Controller** → solo lógica de interacción (ya no ejecuta lógica de negocio directamente).
* **Presenter** → traduce datos crudos en respuestas adecuadas para la vista (JSON, errores, etc.).
* **Repository** y **Model** se mantienen igual.

---

### 📁 Cambios en la estructura

```
app/
├── presenters/
│   ├── user_presenter.py
│   └── task_presenter.py
```

---

### 🧩 Ejemplo de Presenter

#### `user_presenter.py`

```python
from app.models.user_model import User

def present_user(user: User):
    return {"id": user.id, "name": user.name, "email": user.email}

def present_not_found():
    return {"error": "Usuario no encontrado"}
```

#### 🔄 Cambios en `user_controller.py`

```python
from app.repositories.user_repository import UserRepository
from app.models.user_model import User
from app.presenters import user_presenter

user_repo = UserRepository()

def get_user(user_id: int):
    user = user_repo.get_by_id(user_id)
    return user_presenter.present_user(user) if user else user_presenter.present_not_found()
```

---

## ✅ Siguientes pasos para ti

1. Instala dependencias:

   ```bash
   pip install -r requirements.txt
   ```

2. Crea la BD y tablas (si no existen):

   ```sql
   CREATE DATABASE mi_basededatos;
   -- Y usa las tablas que mencionamos antes.
   ```

3. Ejecuta el servidor:

   ```bash
   uvicorn app.main:app --reload
   ```

---

¿Quieres que te genere un archivo ZIP con ambos enfoques (MVC y MCP) listos para probar? También puedo ayudarte a añadir autenticación, validaciones o migraciones si lo deseas.
