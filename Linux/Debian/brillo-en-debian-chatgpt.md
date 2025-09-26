Hola 🙂

Lo que describes en **Debian 13** (y también pasa en versiones anteriores como Debian 12, Ubuntu, Fedora, etc.) no es necesariamente un error, sino una **limitación en cómo el sistema maneja el brillo al reiniciar**:

* El **control de brillo** normalmente lo maneja el **kernel + ACPI** (la interfaz entre Linux y el firmware/BIOS de tu laptop).
* Muchos equipos no almacenan el brillo “cero absoluto” en la NVRAM/firmware, sino que siempre encienden con un **valor mínimo de seguridad** (para que la pantalla no quede completamente negra al arrancar).
* Luego, el entorno gráfico (GNOME, KDE, etc.) puede restaurar el brillo al valor que usabas, pero esto ocurre **después del inicio de sesión**.

👉 Por eso al encender/reiniciar tu Nitro 5 con Debian 13, ves que el brillo está un poco más alto de lo que lo dejaste, aunque luego puedes bajarlo de nuevo.

### Qué puedes hacer:

1. **Verificar si GNOME está restaurando brillo al iniciar sesión**

   * Ajusta el brillo al mínimo.
   * Cierra sesión y vuelve a entrar.
   * Si al entrar se restaura al mínimo, entonces funciona bien, pero al **reiniciar desde cero** vuelve al valor mínimo seguro del firmware.

2. **Guardar el brillo con systemd**
   Puedes usar un servicio para que el sistema recuerde el brillo al apagar y lo restaure al encender:

   ```bash
   sudo systemctl enable systemd-backlight@backlight:acpi_video0.service
   ```

   (en algunos equipos el dispositivo es `intel_backlight` en lugar de `acpi_video0`, se puede ver con `ls /sys/class/backlight/`).

3. **Script manual de restauración**
   Crear un archivo en `/etc/rc.local` o un servicio systemd que escriba el valor en:

   ```
   /sys/class/backlight/intel_backlight/brightness
   ```

   para forzar el nivel que prefieras en cada arranque.

---

🔎 En conclusión: **es normal que no se guarde el brillo en 0 absoluto después de reiniciar**, porque el firmware siempre enciende con un mínimo visible.
No es un fallo de Debian, sino una mezcla de seguridad + cómo el kernel maneja ACPI.

¿Quieres que te prepare un **servicio systemd sencillo** que fije tu nivel de brillo automáticamente en cada arranque?
