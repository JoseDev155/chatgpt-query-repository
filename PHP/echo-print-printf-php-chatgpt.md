¡Hola! En PHP, `echo`, `print` y `printf` son funciones (o estructuras del lenguaje) que se utilizan para **mostrar contenido en pantalla**, pero tienen **diferencias sutiles** en funcionalidad y uso. Aquí te explico cada una con ejemplos:

---

### ✅ `echo`

* **Tipo**: **Construcción del lenguaje**, no una función.
* **Retorna valor**: ❌ No retorna nada.
* **Usos**: Imprimir uno o varios valores rápidamente.
* **Ventaja**: Más rápida y ligera que `print` o `printf`.

```php
<?php
echo "Hola mundo";
echo "<br>";
echo "Nombre: ", "Juan", "<br>"; // puede imprimir múltiples valores separados por comas
?>
```

---

### ✅ `print`

* **Tipo**: **Construcción del lenguaje**, como `echo`.
* **Retorna valor**: ✅ Siempre devuelve `1`.
* **Usos**: Similar a `echo`, pero solo puede imprimir **una sola cadena**.
* **Ventaja**: Se puede usar en expresiones por su valor de retorno.

```php
<?php
print "Hola mundo";
print "<br>";
$exito = print "Mensaje enviado<br>"; // $exito vale 1
echo "¿Se imprimió?: " . $exito;
?>
```

---

### ✅ `printf`

* **Tipo**: **Función**.
* **Retorna valor**: ✅ Devuelve el número de caracteres impresos.
* **Usos**: Imprimir texto **formateado**, al estilo de `printf` en C.
* **Ventaja**: Ideal para formatear números, strings, decimales, etc.

```php
<?php
$nombre = "Juan";
$edad = 30;
$altura = 1.75;

printf("Nombre: %s<br>", $nombre); // %s = string
printf("Edad: %d años<br>", $edad); // %d = entero
printf("Altura: %.2f metros<br>", $altura); // %.2f = número con 2 decimales
?>
```

---

### 📌 ¿Cuándo usar cada una?

| Función  | Cuándo usarla                                                                                                                 |
| -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `echo`   | Para mostrar datos de forma rápida y simple, especialmente si no necesitas formateo.                                          |
| `print`  | Cuando quieras imprimir un solo valor y quizás usar su valor de retorno en una expresión.                                     |
| `printf` | Cuando necesites mostrar **texto formateado**, por ejemplo, para alinear, truncar o mostrar números con precisión específica. |

---

¿Quieres que también te muestre `sprintf`, que es como `printf` pero devuelve la cadena en lugar de imprimirla?
