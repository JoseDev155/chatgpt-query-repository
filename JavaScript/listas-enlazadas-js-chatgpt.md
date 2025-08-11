JavaScript no tiene una implementación nativa de listas enlazadas, pero puedes simular una estructura similar usando objetos. Al igual que en Java, cada nodo de una lista enlazada en JavaScript puede contener un valor y una referencia al siguiente nodo.

Aquí tienes una implementación básica de una lista enlazada en JavaScript:

```javascript
// Clase Nodo
class Nodo {
    constructor(dato) {
        this.dato = dato;
        this.siguiente = null;
    }
}

// Clase ListaEnlazada
class ListaEnlazada {
    constructor() {
        this.cabeza = null;
    }

    // Método para insertar al inicio
    insertarAlInicio(dato) {
        const nuevoNodo = new Nodo(dato);
        nuevoNodo.siguiente = this.cabeza;
        this.cabeza = nuevoNodo;
    }

    // Método para insertar al final
    insertarAlFinal(dato) {
        const nuevoNodo = new Nodo(dato);
        if (this.cabeza === null) {
            this.cabeza = nuevoNodo;
        } else {
            let actual = this.cabeza;
            while (actual.siguiente !== null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevoNodo;
        }
    }

    // Método para mostrar la lista
    mostrarLista() {
        let actual = this.cabeza;
        let listaStr = '';
        while (actual !== null) {
            listaStr += actual.dato + ' -> ';
            actual = actual.siguiente;
        }
        console.log(listaStr + 'null');
    }
}

// Uso de la lista enlazada
const lista = new ListaEnlazada();
lista.insertarAlInicio(10);
lista.insertarAlInicio(20);
lista.insertarAlFinal(30);

lista.mostrarLista();
```

### Explicación

1. **Clase Nodo**: Cada nodo contiene un valor (`dato`) y una referencia (`siguiente`) al siguiente nodo.
2. **Clase ListaEnlazada**: 
   - **`insertarAlInicio(dato)`** agrega un nuevo nodo al comienzo de la lista.
   - **`insertarAlFinal(dato)`** agrega un nuevo nodo al final de la lista.
   - **`mostrarLista()`** recorre la lista y muestra los valores de cada nodo en la consola.

### Consideraciones y Uso en JavaScript

- Las listas enlazadas en JavaScript no son tan comunes como en otros lenguajes de programación como Java o C++ debido a la optimización de arrays nativos de JavaScript.
- **Cuándo usar una lista enlazada en JavaScript**: Si necesitas realizar muchas inserciones y eliminaciones en posiciones arbitrarias de la lista, las listas enlazadas pueden ofrecer una ventaja, aunque esto es raro. Sin embargo, en la mayoría de los casos, los arrays de JavaScript son muy eficientes y flexibles.

Esta estructura de lista enlazada en JavaScript ofrece una manera de gestionar datos en secuencia de forma dinámica y puede ser útil si quieres practicar o implementar estructuras de datos manualmente.
