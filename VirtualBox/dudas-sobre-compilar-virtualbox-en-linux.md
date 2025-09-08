Muy buena duda 👌 porque aquí entran en juego dos cosas distintas: **dónde compilas** y **dónde se instala**.

---

## 🔹 1. Ruta donde compilas

No importa si descomprimes y compilas en:

```
/home/user/Downloads/VirtualBox-<versión>/
```

o en

```
/opt/src/VirtualBox-<versión>/
```

La compilación siempre ocurre en esa carpeta, pero eso **no significa que se instale ahí**.
Lo que obtienes en esa ruta son binarios compilados y librerías listas, pero aún no "instaladas" en el sistema.

---

## 🔹 2. Ruta donde se instala

El **Makefile/configure de VirtualBox** está preparado para instalar los binarios en rutas estándar de Linux, típicamente:

* Ejecutables → `/usr/lib/virtualbox/`
* Frontend (`VirtualBox`, `VBoxManage`, etc.) → symlinks en `/usr/bin/`
* Archivos de servicio → `/etc/`

📌 Esto lo controla la fase `make install`, no la compilación.

Por defecto, VirtualBox usa `/usr` como **prefix** de instalación.
Pero puedes cambiarlo al configurar la build, con algo como:

```bash
./configure --prefix=/opt/virtualbox
```

y entonces todo irá a `/opt/virtualbox` en vez de `/usr/lib/virtualbox`.

---

## 🔹 3. Resumen

* **Compilación**: se hace donde tengas el código fuente, no ensucia tu sistema.
* **Instalación**: tiene sus rutas predefinidas (usualmente `/usr/lib/virtualbox/` y symlinks en `/usr/bin/`).
* Puedes cambiar esas rutas con `--prefix` al correr `./configure`.

---

👉 Entonces, si compilas en `/home/user/Downloads/VirtualBox-*`, los binarios intermedios estarán ahí, pero cuando hagas `sudo make install`, se copiarán al sistema en `/usr/lib/virtualbox` (a menos que uses `--prefix`).

---

¿Quieres que te arme un **ejemplo paso a paso** para compilar VirtualBox en Debian 13 con `--prefix=/opt/virtualbox`, de modo que quede aislado del VirtualBox que viene en los repos oficiales?
