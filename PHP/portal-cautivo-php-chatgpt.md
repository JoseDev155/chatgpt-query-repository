Un **portal cautivo** es una página web que se muestra automáticamente cuando un usuario intenta acceder a Internet a través de una red Wi-Fi pública. Es común en hoteles, cafeterías, aeropuertos, etc. Su propósito principal es **autenticar** al usuario, mostrar términos y condiciones o solicitar algún tipo de acción (como ingresar una contraseña o registrarse) antes de permitir el acceso a la red.

---

### 🚀 **¿Cómo funciona?**

1. **Conexión inicial:**  
   El usuario se conecta a la red Wi-Fi.

2. **Redirección automática:**  
   Cualquier intento de abrir un sitio web (ej., `google.com`) redirige al portal cautivo en lugar de la página deseada. Esto ocurre porque el servidor intercepta la solicitud HTTP/HTTPS.

3. **Autenticación/Acción del usuario:**  
   El usuario completa un formulario en el portal cautivo, como ingresar un nombre de usuario y contraseña, aceptar términos o registrarse.

4. **Acceso permitido:**  
   Si la autenticación es correcta, el servidor libera el acceso a Internet.

---

### 🛠️ **Implementación básica con PHP**

Puedes crear un portal cautivo básico con PHP utilizando los siguientes pasos:

1. **Configura tu red para redirigir el tráfico**  
   - Usa un **servidor DNS** y un **servidor web** (como Apache o Nginx) para redirigir todo el tráfico a tu portal cautivo.

2. **Crea una página de inicio de sesión (portal.php):**

```php
<?php
// portal.php

// Simulamos un formulario de login
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $password = $_POST["password"];

    // Verificar credenciales (básico)
    if ($username == "admin" && $password == "1234") {
        // Marca al usuario como autenticado (cookie o IP permitida)
        setcookie("authenticated", "true", time() + 3600); // 1 hora
        header("Location: success.php");
        exit;
    } else {
        $error = "Credenciales incorrectas.";
    }
}
?>

<!DOCTYPE html>
<html lang="es">
<head>
    <title>Portal Cautivo</title>
</head>
<body>
    <h2>Bienvenido al Portal Cautivo</h2>
    <?php if (isset($error)) echo "<p style='color:red;'>$error</p>"; ?>
    <form method="POST" action="portal.php">
        <label>Usuario:</label><br>
        <input type="text" name="username" required><br>
        <label>Contraseña:</label><br>
        <input type="password" name="password" required><br><br>
        <input type="submit" value="Acceder">
    </form>
</body>
</html>
```

3. **Página de éxito (success.php):**

```php
<?php
// success.php
if (!isset($_COOKIE["authenticated"])) {
    header("Location: portal.php");
    exit;
}
?>

<!DOCTYPE html>
<html lang="es">
<head>
    <title>Acceso Permitido</title>
</head>
<body>
    <h2>¡Acceso concedido!</h2>
    <p>Ya puedes navegar por Internet.</p>
</body>
</html>
```

4. **Redirige el tráfico no autenticado a `portal.php`:**  
   Antes de permitir navegar, verifica si el usuario está autenticado usando cookies, sesiones o verificando su IP en una lista de control.

---

### 🔧 **Pasos adicionales importantes:**

1. **Interceptar tráfico:**  
   Usa herramientas como `iptables` en Linux o configuraciones específicas en tu router para redirigir todo el tráfico HTTP al servidor web donde se ejecuta tu portal.

2. **Liberar acceso:**  
   Una vez autenticado, configura reglas en tu servidor para permitir el tráfico del usuario a Internet.

3. **Seguridad:**  
   - Asegura la transmisión de datos usando HTTPS.  
   - Valida correctamente las credenciales.  
   - Controla los tiempos de sesión (para desconectar usuarios después de un tiempo).

---

Con estos pasos básicos y PHP puedes implementar un portal cautivo funcional. Para una solución más robusta, se recomienda combinar PHP con herramientas de red avanzadas como **FreeRADIUS** o **ChilliSpot**.