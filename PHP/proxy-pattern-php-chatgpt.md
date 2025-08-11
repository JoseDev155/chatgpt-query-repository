Aquí tienes un programa en PHP que gestiona usuarios aplicando el patrón de diseño Proxy, utilizando PDO para PostgreSQL y el patrón MVC. La estructura del proyecto será la siguiente:

```
project/
│
├── config/
│   └── Database.php
│
├── models/
│   ├── IUser.php
│   ├── User.php
│   └── UserProxy.php
│
├── controllers/
│   └── UserController.php
│
└── index.php
```

### 1. **Archivo de configuración de la base de datos (Database.php):**
```php
<?php
namespace Config;

use PDO;
use PDOException;

class Database
{
    private $host = 'localhost';
    private $dbname = 'your_database';
    private $username = 'your_username';
    private $password = 'your_password';
    private static $connection;

    public static function getConnection()
    {
        if (!self::$connection) {
            try {
                self::$connection = new PDO(
                    "pgsql:host=localhost;dbname=your_database",
                    'your_username',
                    'your_password',
                    [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]
                );
            } catch (PDOException $e) {
                die("Error connecting to database: " . $e->getMessage());
            }
        }
        return self::$connection;
    }
}
```

### 2. **Interfaz para el modelo de usuario (IUser.php):**
```php
<?php
namespace Models;

interface IUser
{
    public function getUsers();
}
```

### 3. **Clase principal de usuario (User.php):**
```php
<?php
namespace Models;

use Config\Database;

class User implements IUser
{
    public function getUsers()
    {
        $db = Database::getConnection();
        $stmt = $db->query("SELECT id, name, email FROM users");
        return $stmt->fetchAll(PDO::FETCH_ASSOC);
    }
}
```

### 4. **Clase Proxy de usuario (UserProxy.php):**
```php
<?php
namespace Models;

class UserProxy implements IUser
{
    private $realUser;
    private $cachedUsers = null;

    public function __construct(User $user)
    {
        $this->realUser = $user;
    }

    public function getUsers()
    {
        if ($this->cachedUsers === null) {
            $this->cachedUsers = $this->realUser->getUsers();
        }
        return $this->cachedUsers;
    }
}
```

### 5. **Controlador para manejar usuarios (UserController.php):**
```php
<?php
namespace Controllers;

use Models\User;
use Models\UserProxy;

class UserController
{
    public function index()
    {
        $realUser = new User();
        $userProxy = new UserProxy($realUser);

        $users = $userProxy->getUsers();
        header('Content-Type: application/json');
        echo json_encode($users);
    }
}
```

### 6. **Punto de entrada principal (index.php):**
```php
<?php

require_once __DIR__ . '/config/Database.php';
require_once __DIR__ . '/models/IUser.php';
require_once __DIR__ . '/models/User.php';
require_once __DIR__ . '/models/UserProxy.php';
require_once __DIR__ . '/controllers/UserController.php';

use Controllers\UserController;

$controller = new UserController();
$controller->index();
```

### Explicación del funcionamiento:
1. **`Database.php`**: Se encarga de gestionar la conexión con la base de datos PostgreSQL.
2. **`IUser.php`**: Define una interfaz común para las clases `User` y `UserProxy`.
3. **`User.php`**: Realiza la consulta real a la base de datos.
4. **`UserProxy.php`**: Actúa como intermediario para almacenar en caché los usuarios recuperados, reduciendo la cantidad de llamadas a la base de datos.
5. **`UserController.php`**: Orquesta la lógica de negocio y devuelve los datos en formato JSON.
6. **`index.php`**: Sirve como punto de entrada del programa y llama al controlador.

Con esta estructura puedes gestionar usuarios de manera eficiente, utilizando el patrón Proxy y aplicando el patrón MVC en PHP. Si necesitas personalizar algo, ¡házmelo saber!