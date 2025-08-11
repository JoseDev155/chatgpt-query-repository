Para vaciar el contenido de un archivo en **CMD**, puedes usar el siguiente comando:

```cmd
echo. > archivo.txt
```

### Explicación:
1. **`echo.`**: Genera una línea vacía.
2. **`>`**: Redirige la salida al archivo, sobrescribiendo su contenido.

Esto dejará el archivo `archivo.txt` completamente vacío. Si necesitas asegurarte de que el archivo exista antes de vaciarlo, puedes agregar una comprobación:

```cmd
if exist archivo.txt (echo. > archivo.txt) else (echo El archivo no existe.)
```

Cualquier duda adicional, ¡avísame! 😊