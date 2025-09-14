¡Bien! 🔥 Eso confirma la sospecha: el problema no era que te faltara borrar más cosas del Registro o ProgramData, sino que el instalador detectaba **archivos de instalación/actualización pendientes en la carpeta temporal de Windows**.

Lo que pasó es más o menos así:

1. Cuando desinstalaste AutoCAD, **el instalador dejó archivos en `%localappdata%\Temp`** (o en `%localappdata%\Autodesk\ODIS`).

   * Estos archivos incluyen marcadores de *pending reboot* o scripts de actualización interrumpidos.
   * Ejemplo: `AutoCAD-XXXX-UPDATE.log` o `*.msi` a medio instalar.

2. Al volver a instalar, el instalador de AutoCAD escaneó esa carpeta y encontró rastros de una actualización inconclusa.

   * Eso le hizo asumir que **Windows todavía requería un reinicio** para aplicar cambios.

3. Como nunca se eliminaron esos temporales, el instalador quedó atrapado en un “loop” de pedir reinicio aunque ya no había nada realmente bloqueando el sistema.

4. Cuando borraste esos archivos, **se eliminó la marca de actualización pendiente** y el instalador volvió a funcionar normalmente.

---

👉 En resumen:
El error se debió a que **los logs y temporales de instalación de AutoCAD guardaban un estado “pendiente de reinicio”**, y al no borrarlos, el instalador lo seguía detectando cada vez.

---

¿Quieres que te dé un **par de recomendaciones prácticas** para que no te vuelva a pasar con otros programas de Autodesk (AutoCAD, Revit, Maya, etc.) cuando desinstales/reinstales?
