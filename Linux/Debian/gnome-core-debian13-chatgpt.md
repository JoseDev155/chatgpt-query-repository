En tu caso, si ya tienes Debian 13 sin entorno gráfico y quieres instalar **solo** el entorno base de GNOME (sin todo el paquete “recomendado” lleno de juegos y aplicaciones extra), el comando sería:

```bash
sudo apt update
sudo apt install gnome-core
```

Esto instalará:

* El **shell** de GNOME
* Configuración y utilidades esenciales
* Gestor de inicio de sesión **GDM**

💡 Si quieres además asegurarte de que se instale el servidor gráfico y algunas herramientas mínimas para que todo arranque correctamente, puedes hacer:

```bash
sudo apt install xorg gnome-core gdm3
```

Cuando reinicies, ya deberías entrar a GNOME minimal sin el “bloatware” que viene con `gnome` o `task-gnome-desktop`.

Si en algún momento quieres **el GNOME completo** con LibreOffice, juegos y utilidades extra, ese sería:

```bash
sudo apt install gnome
```

o

```bash
sudo apt install task-gnome-desktop
```

¿Quieres que te ponga una tabla comparativa rápida entre `gnome`, `gnome-core` y `task-gnome-desktop` para que veas las diferencias de paquetes?
