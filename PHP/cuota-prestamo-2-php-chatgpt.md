Aquí tienes un **script PHP completo** que permite calcular la **prima necesaria** para alcanzar una **cuota mensual deseada**, usando los mismos parámetros de interés, plazo y precio del vehículo:

```php
<?php
// Parámetros del vehículo y préstamo
$valorVehiculo = 30000;           // Precio total del vehículo
$interesAnual = 9.95;             // Tasa de interés anual en %
$plazoMeses = 100;                // Plazo en meses

// Tasa de interés mensual
$tasaMensual = ($interesAnual / 100) / 12;

// Cuotas mensuales deseadas (puedes agregar más)
$cuotasDeseadas = [350, 400];

// Cálculo de (1 + r)^n y otras constantes
$factorPotencia = pow(1 + $tasaMensual, $plazoMeses);
$factorNumerador = $factorPotencia - 1;
$factorDenominador = $tasaMensual * $factorPotencia;

// Función para calcular el monto financiado según la cuota deseada
function calcularMontoFinanciado($cuotaDeseada, $numerador, $denominador) {
    return $cuotaDeseada * ($numerador / $denominador);
}

// Mostrar resultados
foreach ($cuotasDeseadas as $cuota) {
    $montoFinanciado = calcularMontoFinanciado($cuota, $factorNumerador, $factorDenominador);
    $primaRequerida = $valorVehiculo - $montoFinanciado;

    echo "--------------------------------------------\n";
    printf("Cuota deseada: \$%.2f\n", $cuota);
    printf("Monto financiado: \$%.2f\n", $montoFinanciado);
    printf("Prima requerida: \$%.2f\n", $primaRequerida);
}
echo "--------------------------------------------\n";
?>
```

---

### 🟢 ¿Cómo usarlo?

1. Guarda este código como `calculo_prima.php`.
2. Ejecuta desde la terminal con:

```bash
php calculo_prima.php
```

---

¿Te gustaría una versión interactiva por consola o mediante formulario web?
