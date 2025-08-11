Aquí tienes un comando en **CMD** que realiza lo que necesitas, además con la opción de cambiar la ruta y el archivo de salida dinámicamente:

```cmd
@echo off
set /p ruta=Ingresa la ruta para generar la estructura de directorios: 
set /p archivo=Ingresa el nombre del archivo de salida (con .txt): 
tree "%ruta%" /f > "%archivo%"
echo Estructura de directorios guardada en %archivo%.
pause
```

### Explicación:
1. **`set /p ruta`**: Pide al usuario la ruta para listar la estructura de directorios.
2. **`set /p archivo`**: Pide al usuario el nombre del archivo donde se guardará la estructura.
3. **`tree "%ruta%" /f > "%archivo%"`**: Genera la estructura de directorios con archivos y la guarda en el archivo especificado.
4. **`pause`**: Pausa la ventana para que el usuario pueda ver el mensaje final.

Copia este código en un archivo de texto y guárdalo con la extensión `.bat` (por ejemplo, `guardar_estructura.bat`). Al ejecutarlo, pedirá la ruta y el archivo de salida.

Si necesitas que haga algo más, avísame. 😊