Aquí tienes un script PHP sencillo para calcular la **cuota mensual**, el **total pagado**, y los **intereses** de un préstamo:

```php
<?php
// Datos del préstamo
$precioVehiculo = 30000;        // Monto del préstamo en dólares
$interesAnual = 9.95;           // Interés anual en porcentaje
$plazoMeses = 100;              // Número de meses

// Convertir interés anual a tasa mensual decimal
$tasaMensual = ($interesAnual / 100) / 12;

// Fórmula de amortización francesa para calcular cuota fija mensual
$factor = pow(1 + $tasaMensual, $plazoMeses);
$cuotaMensual = $precioVehiculo * ($tasaMensual * $factor) / ($factor - 1);

// Total pagado y total de intereses
$totalPagado = $cuotaMensual * $plazoMeses;
$interesesTotales = $totalPagado - $precioVehiculo;

// Mostrar resultados con formato
printf("Cuota mensual: \$%.2f\n", $cuotaMensual);
printf("Total pagado: \$%.2f\n", $totalPagado);
printf("Total de intereses: \$%.2f\n", $interesesTotales);
?>
```

---

### 🟢 ¿Cómo usarlo?

Guarda el código en un archivo como `calculo_prestamo.php` y ejecútalo con:

```bash
php calculo_prestamo.php
```

Este script también puede adaptarse fácilmente para tomar entradas del usuario por consola o por formulario web si lo necesitas.

¿Te gustaría una versión que reciba los datos por formulario HTML?
