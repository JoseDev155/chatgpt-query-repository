### ¿Qué es TDD?

TDD (Test-Driven Development o Desarrollo Guiado por Pruebas) es una metodología de desarrollo de software en la que se escriben pruebas antes de escribir el código funcional. Su flujo básico es:

1. **Escribir una prueba**: Crear una prueba unitaria que defina lo que el código debe hacer.
2. **Ejecutar la prueba**: Verificar que falle (porque aún no existe el código funcional).
3. **Escribir el código**: Implementar la funcionalidad mínima necesaria para que la prueba pase.
4. **Ejecutar la prueba nuevamente**: Confirmar que ahora pasa correctamente.
5. **Refactorizar**: Mejorar el código si es necesario, manteniendo las pruebas exitosas.

Esto asegura que el código sea funcional, mantenible y cumpla con los requisitos desde el principio.

---

### Proyecto de ejemplo con TDD en Java

Crearemos un proyecto para calcular el área de un rectángulo. Usaremos TDD para guiar el desarrollo.

---

#### Paso 1: Estructura del proyecto
Imagina que tenemos un proyecto Maven con esta estructura:

```
tdd-example/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com.example/
│   │           └── Rectangle.java
│   ├── test/
│       └── java/
│           └── com.example/
│               └── RectangleTest.java
└── pom.xml
```

---

#### Paso 2: Escribir la prueba (primero)
En `RectangleTest.java`, escribimos la prueba inicial:

```java
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class RectangleTest {

    @Test
    public void testCalculateArea() {
        Rectangle rectangle = new Rectangle(5, 10); // Crear un rectángulo de 5x10
        assertEquals(50, rectangle.calculateArea()); // El área debería ser 50
    }
}
```

Aquí usamos JUnit 5. La prueba fallará porque no hemos implementado la clase `Rectangle`.

---

#### Paso 3: Implementar el código mínimo
Creamos la clase `Rectangle.java` para que la prueba pase:

```java
package com.example;

public class Rectangle {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public int calculateArea() {
        return width * height; // Fórmula básica para el área
    }
}
```

---

#### Paso 4: Ejecutar la prueba nuevamente
Ejecuta la prueba con JUnit. Ahora debería pasar porque hemos implementado la funcionalidad.

---

#### Paso 5: Refactorizar
El código es suficientemente simple, pero si encontráramos redundancias o problemas, aquí los corregiríamos.

---

### Resultado
Ahora tenemos una funcionalidad bien probada para calcular el área de un rectángulo, desarrollada siguiendo TDD.

Si quieres más ejemplos o extender este, ¡dímelo! 😊