En C, un archivo con extensión **`.h`** es un **archivo de encabezado** (header file) que contiene declaraciones de funciones, macros, estructuras y variables globales que pueden ser utilizadas en múltiples archivos `.c`.  

Puedes generar un archivo `.h` de varias formas:  

### **📌 Opción 1: Crear un archivo manualmente**  
1. **Abre un editor de texto o IDE** (como VS Code, Vim, Nano, etc.).  
2. **Crea un nuevo archivo** y guárdalo con la extensión `.h`, por ejemplo: `usuario.h`.  
3. **Escribe las declaraciones** dentro del archivo.  

Ejemplo de un **archivo de encabezado (`usuario.h`)**:  
```c
#ifndef USUARIO_H    // Verifica si USUARIO_H no ha sido definido
#define USUARIO_H    // Lo define para evitar múltiples inclusiones

// Definición de la estructura Usuario
typedef struct {
    char nombre[50];
    int edad;
} Usuario;

// Declaración de funciones
void mostrarUsuario(Usuario u);

#endif  // USUARIO_H
```

---

### **📌 Opción 2: Crear un archivo `.h` con un editor desde la terminal**  
#### **📂 Crear el archivo en Linux/Mac**  
En la terminal, usa:  
```sh
touch usuario.h
nano usuario.h
```
Esto abre el editor `nano` donde puedes escribir el contenido y guardar (`Ctrl + X`, luego `Y`, luego `Enter`).  

#### **📂 Crear el archivo en Windows (CMD o PowerShell)**  
En **CMD**:
```cmd
echo // Archivo de encabezado vacío > usuario.h
```
En **PowerShell**:
```powershell
New-Item usuario.h
```

---

### **📌 Cómo Usar el Archivo `.h` en un Programa**  
Después de crearlo, puedes **importarlo en un archivo `.c`** con `#include "usuario.h"`.

#### **Ejemplo Completo**
##### **Archivo `usuario.h` (Encabezado)**
```c
#ifndef USUARIO_H
#define USUARIO_H

typedef struct {
    char nombre[50];
    int edad;
} Usuario;

void mostrarUsuario(Usuario u);

#endif
```

##### **Archivo `usuario.c` (Implementación)**
```c
#include <stdio.h>
#include "usuario.h"

void mostrarUsuario(Usuario u) {
    printf("Nombre: %s, Edad: %d\n", u.nombre, u.edad);
}
```

##### **Archivo `main.c` (Programa Principal)**
```c
#include <stdio.h>
#include "usuario.h"

int main() {
    Usuario u = {"Carlos", 25};
    mostrarUsuario(u);
    return 0;
}
```

### **🏗 Cómo Compilar y Ejecutar**  
Ejecuta en la terminal:  
```sh
gcc main.c usuario.c -o programa
./programa
```

---

### **📌 Resumen**  
✅ Un **archivo `.h`** contiene declaraciones de funciones y estructuras.  
✅ Se crea manualmente o con comandos como `touch usuario.h`.  
✅ Se incluye en los archivos `.c` con `#include "archivo.h"`.  
✅ Usa `#ifndef ... #define ... #endif` para evitar múltiples inclusiones.  

Si tienes más dudas, dime. ¡Te ayudo! 😊