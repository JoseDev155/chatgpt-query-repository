¡Exacto! 💯 Esa combinación es muy práctica y de hecho muchos lo hacen así para evitar choques de dependencias.

---

## 🔹 Esquema recomendado

1. **Scala 2.\*** (para Spark)

   * Lo instalas en **`/usr/lib/scala-2.12/`** (o `/usr/lib/scala-2.13/` según la versión de tu Spark).
   * Creas un symlink fijo en `/usr/local/bin/scala` que apunte a esa versión **solo para Spark**.
   * Spark sabrá usar esa versión sin problemas porque es estable y no cambiará a menos que actualices Spark.

   Ejemplo:

   ```bash
   sudo tar -xvzf scala-2.12.18.tgz -C /usr/lib/
   sudo ln -s /usr/lib/scala-2.12.18/bin/scala /usr/local/bin/scala2
   sudo ln -s /usr/lib/scala-2.12.18/bin/scalac /usr/local/bin/scalac2
   ```

   (Así no ensucias el `scala` por defecto y lo llamas como `scala2` si lo necesitas).

---

2. **Scala (última, con Coursier)**

   * Usas **Coursier (`cs`)** para manejar Scala 3 (o múltiples versiones) en `~/.local/share/coursier/bin/`.
   * Esto te permite:

     * Probar nuevas features de Scala 3.
     * Instalar herramientas como **sbt**, **ammonite**, **scala-cli** fácilmente.
     * Cambiar de versión sin tocar el entorno de Spark.

   Ejemplo:

   ```bash
   cs install scala3
   cs install sbt
   ```

   (Quedará en tu `$HOME`, sin chocar con la instalación de `/usr/lib`).

---

## 🔹 Ventajas de esta mezcla

* **Spark** siempre usa una versión fija (Scala 2.12/2.13 en `/usr/lib`).
* **Tú** puedes experimentar y desarrollar con Scala 3 (u otras versiones) sin miedo a romper Spark.
* Mantienes separadas las rutas:

  * Sistema (`/usr/lib/scala-*`) → estable
  * Usuario (`~/.local/share/coursier/bin/`) → flexible

---

👉 Yo lo veo como tener **Java 8 en `/usr/lib/jvm/` para apps legacy** y **Java 21 con SDKMAN o tarball** para desarrollo moderno: convivencia sin choque.

---

¿Quieres que te arme un **paso a paso** de cómo dejar ese setup mixto (Spark con Scala 2.12 en `/usr/lib/` + Scala 3 con Coursier en `~/.local/`), incluyendo la configuración del `$PATH` para alternar entre ambos?
