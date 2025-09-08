¡Buena pregunta! 🚀 Scala en Linux (incluyendo Debian 13) se puede instalar de varias formas, y depende de lo que priorices: facilidad de actualización, control de versiones, o tener una instalación "del sistema".

---

## 🔹 1. Instalación con **Coursier CLI (recomendada)**

* **Coursier (`cs`)** es hoy la forma oficial y más óptima de instalar Scala, SBT y herramientas relacionadas, ya que gestiona versiones, dependencias y actualizaciones de manera automática.
* Te instala **Scala CLI**, **sbt**, **Ammonite**, y más con un solo comando.
* Además, no ensucia `/usr/lib` ni `/usr/local`, sino que sigue una lógica más de tipo "gestor de paquetes de usuario".

### 📌 Ruta de instalación por defecto de Coursier

Por defecto, cuando corres:

```bash
curl -fL https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz | gzip -d > cs
chmod +x cs
./cs install scala sbt
```

Coursier instala los binarios en:

* `~/.local/share/coursier/bin`  → ejecutables (ej: `scala`, `sbt`, `scalac`, etc.)
* `~/.cache/coursier`  → dependencias y jars descargados

Si tienes `~/.local/share/coursier/bin` en tu `$PATH`, puedes ejecutar `scala` y `sbt` directamente.

---

## 🔹 2. Instalación manual con archivo `.tar.gz`

Si prefieres descargar el tarball oficial de [scala-lang.org](https://www.scala-lang.org/download/), obtendrás solo el compilador (`scala`, `scalac`) sin integración automática de SBT ni gestión de dependencias.

### 📌 Ubicaciones recomendadas

En este caso, lo tratas como una instalación de software "del sistema". Las ubicaciones recomendadas serían:

* **/usr/lib/scala/** → similar a cómo Debian organiza Java en `/usr/lib/jvm/`
* **/opt/scala/** → alternativa si quieres mantenerlo aislado, más común para software instalado manualmente
* **/usr/local/share/scala/** → también válido para instalaciones manuales globales

Luego solo necesitas crear symlinks en `/usr/local/bin/` para `scala` y `scalac`, por ejemplo:

```bash
sudo ln -s /usr/lib/scala/bin/scala /usr/local/bin/scala
sudo ln -s /usr/lib/scala/bin/scalac /usr/local/bin/scalac
```

---

## 🔹 Comparación final

* **Coursier CLI** → ✔️ más óptimo si quieres siempre tener la versión más reciente, actualizaciones fáciles, múltiples versiones y herramientas extra.
* **tar.gz en `/usr/lib/scala` o `/opt/scala`** → ✔️ más tradicional y controlado, útil si quieres algo "estable del sistema" sin gestor externo.

---

👉 En resumen:

* Si lo usarás para proyectos en constante cambio, aprendizaje, o quieres algo al estilo `nvm`/`pyenv`, usa **Coursier**.
* Si lo quieres fijo y gestionado como Java en tu sistema, usa **tar.gz** y ponlo en `/usr/lib/scala` o `/opt/scala`.

---

¿Quieres que te arme un ejemplo de instalación **mixta**? (Coursier para desarrollo y tar.gz en `/usr/lib/scala` como respaldo del sistema).
