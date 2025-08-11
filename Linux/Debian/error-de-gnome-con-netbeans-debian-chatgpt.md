El error que estás viendo ocurre porque el script de instalación de NetBeans está tratando de utilizar una interfaz gráfica, pero no puede encontrar una pantalla X11 (la interfaz gráfica de Linux). Esto suele pasar si estás tratando de ejecutar el script en un entorno sin GUI o por SSH sin reenvío X11 habilitado.

Aquí tienes algunas soluciones posibles:

### 1. Ejecutar el script en un entorno con GUI
Si estás en un entorno de escritorio, asegúrate de estar ejecutando el script desde una terminal en tu entorno gráfico (no desde una terminal virtual como `Ctrl+Alt+F1`).

### 2. Usar reenvío X11 si estás conectado por SSH
Si estás conectado por SSH, asegúrate de que el reenvío X11 está habilitado. Puedes hacerlo agregando la opción `-X` o `-Y` cuando inicies la sesión SSH. Por ejemplo:
```bash
ssh -X usuario@servidor
```

### 3. Configurar la variable DISPLAY manualmente
Asegúrate de que la variable de entorno `DISPLAY` esté configurada correctamente. En una terminal gráfica, puedes configurarla manualmente:
```bash
export DISPLAY=:0
./Apache-NetBeans-13-bin-linux-x64.sh
```

### 4. Usar un entorno virtual de X (Xvfb)
Si no tienes acceso a un entorno gráfico, puedes usar un framebuffer virtual como `Xvfb`. Instala `Xvfb` y ejecuta el script en un entorno virtual de X:
```bash
sudo apt-get install xvfb
Xvfb :1 -screen 0 1024x768x16 &
export DISPLAY=:1
./Apache-NetBeans-13-bin-linux-x64.sh
```

Prueba alguna de estas soluciones y dime si alguna funciona para ti.
