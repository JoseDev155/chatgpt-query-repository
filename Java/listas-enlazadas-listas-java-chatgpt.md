Las **listas enlazadas** son estructuras de datos compuestas por nodos, donde cada nodo almacena un valor y una referencia (o enlace) al siguiente nodo de la lista. A diferencia de las **listas normales** (o arreglos), las listas enlazadas no tienen posiciones contiguas en la memoria, lo que les permite crecer dinámicamente sin necesidad de reubicarse o reasignar memoria.

### Diferencias entre Listas Enlazadas y Listas (Arreglos)

1. **Almacenamiento en Memoria**:
   - **Lista Enlazada**: Los elementos no están en ubicaciones contiguas de memoria. Cada nodo almacena una referencia al siguiente nodo, lo que permite una estructura más flexible y fácil de expandir.
   - **Lista (Arreglo)**: Los elementos están en ubicaciones contiguas de memoria, lo cual facilita el acceso aleatorio, pero requiere una memoria de tamaño fijo o reubicación cuando se excede el tamaño asignado.

2. **Acceso a Elementos**:
   - **Lista Enlazada**: Tiene acceso secuencial. Acceder a un elemento en una posición específica es menos eficiente (O(n)), ya que requiere recorrer los nodos hasta llegar al elemento deseado.
   - **Lista (Arreglo)**: Ofrece acceso aleatorio a cualquier elemento en O(1) debido a las posiciones contiguas.

3. **Inserción y Eliminación**:
   - **Lista Enlazada**: Las operaciones de inserción y eliminación (en posiciones específicas) son eficientes, especialmente al inicio y final (O(1) en esos casos).
   - **Lista (Arreglo)**: Para insertar o eliminar un elemento en posiciones intermedias, es necesario desplazar los elementos, lo cual tiene una complejidad de O(n).

4. **Uso de Memoria**:
   - **Lista Enlazada**: Consume más memoria por el uso de punteros adicionales (enlacenodos).
   - **Lista (Arreglo)**: Usa menos memoria ya que solo almacena los elementos.

### Ejemplo en Java

A continuación, un ejemplo sencillo de una lista enlazada y cómo insertar y mostrar elementos:

```java
// Nodo de la lista enlazada
class Nodo {
    int dato;
    Nodo siguiente;

    public Nodo(int dato) {
        this.dato = dato;
        this.siguiente = null;
    }
}

// Lista enlazada
class ListaEnlazada {
    private Nodo cabeza;

    public ListaEnlazada() {
        this.cabeza = null;
    }

    // Método para insertar al inicio
    public void insertarAlInicio(int dato) {
        Nodo nuevoNodo = new Nodo(dato);
        nuevoNodo.siguiente = cabeza;
        cabeza = nuevoNodo;
    }

    // Método para insertar al final
    public void insertarAlFinal(int dato) {
        Nodo nuevoNodo = new Nodo(dato);
        if (cabeza == null) {
            cabeza = nuevoNodo;
        } else {
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevoNodo;
        }
    }

    // Método para mostrar la lista
    public void mostrarLista() {
        Nodo actual = cabeza;
        while (actual != null) {
            System.out.print(actual.dato + " -> ");
            actual = actual.siguiente;
        }
        System.out.println("null");
    }
}

// Clase principal
public class EjemploListaEnlazada {
    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        
        lista.insertarAlInicio(10);
        lista.insertarAlInicio(20);
        lista.insertarAlFinal(30);

        lista.mostrarLista();
    }
}
```

### Cuándo Usar Listas Enlazadas

Las listas enlazadas son más eficientes en los siguientes casos:
- **Inserciones o eliminaciones frecuentes en posiciones intermedias** o al inicio, ya que no requieren reordenamiento de elementos.
- **Tamaño desconocido o dinámico**: Si no sabes el tamaño final de la lista y necesitas que crezca dinámicamente.

Por otro lado, si necesitas acceder frecuentemente a elementos en posiciones específicas, un arreglo o una lista en Java (`ArrayList`) sería más eficiente debido a su acceso aleatorio en O(1).
