Muy buena observación 👌

Lo que viste en la ArchWiki seguramente está relacionado con las **XDG Base Directory Specification** (el estándar XDG de FreeDesktop). Te resumo:

### 🔹 Qué dice el estándar XDG

El estándar XDG define rutas en las que las aplicaciones **deberían** guardar sus archivos:

* **`$XDG_CONFIG_HOME`** → configuración del usuario (por defecto `~/.config/`).
* **`$XDG_DATA_HOME`** → datos de usuario (por defecto `~/.local/share/`).
* **`$XDG_CACHE_HOME`** → cachés (por defecto `~/.cache/`).
* **`$XDG_STATE_HOME`** → estado de programas (por defecto `~/.local/state/`).

La idea es que en tu `$HOME` no se llene de **dotfiles** (`~/.programrc`, `~/.program/`, etc.), sino que todo se organice bajo `.config`, `.local/`, etc.

---

### 🔹 Lo que realmente pasa en la práctica

* Muchas aplicaciones **ya siguen XDG** (por ejemplo, Firefox, Neovim, Gnome apps, VSCode).
* Otras todavía **usan dotfiles heredados** (`~/.bashrc`, `~/.vimrc`, `~/.m2/`, `~/.cargo/`, etc.).
* Algunas respetan **las variables de entorno XDG** si las defines en `~/.bashrc` o `~/.zshrc`. Ejemplo:

  ```bash
  export XDG_CONFIG_HOME="$HOME/.config"
  export XDG_DATA_HOME="$HOME/.local/share"
  export XDG_CACHE_HOME="$HOME/.cache"
  ```

  Pero no todas las apps obedecen esto.

---

### 🔹 ¿Es mala práctica instalar programas en `$HOME`?

Depende:

* Instalar programas en `$HOME` (por ejemplo `~/.local/bin`) es una **práctica aceptada y común** cuando no tienes permisos de root o quieres un entorno aislado.
* Lo que **sí es mala práctica** es que un programa llene tu `$HOME` con archivos de configuración en lugar de usar `.config/`, `.cache/`, etc. → pero esto es culpa del programa, no tuya.

---

### 🔹 ¿Es viable mover todo a `.config` manualmente?

* Si **el programa soporta XDG** (ejemplo: `neovim` respeta `XDG_CONFIG_HOME`), sí puedes mover su configuración a `.config/nvim` y funcionará.
* Si **el programa NO soporta XDG**, mover sus archivos a `.config` hará que no los encuentre, a menos que uses enlaces simbólicos:

  ```bash
  mv ~/.programrc ~/.config/program/config
  ln -s ~/.config/program/config ~/.programrc
  ```
* Algunos proyectos como [`xdg-ninja`](https://github.com/b3nj5m1n/xdg-ninja) ayudan a detectar qué programas están ensuciando `$HOME`.

---

### 🔹 En Debian 13 en tu equipo

Es **totalmente viable** empezar a usar el estándar XDG:

* Define las variables en tu `.bashrc` o `.zshrc`.
* Configura nuevas aplicaciones directamente en `.config`.
* Para las antiguas, usa symlinks o déjalas como están.

👉 Pero **no intentes forzar todo de golpe**, porque muchos programas legacy no lo soportan bien. Lo mejor es migrar poco a poco.

---

¿Quieres que te prepare un **ejemplo práctico** de cómo podrías configurar tu Debian 13 para empezar a adoptar XDG (con variables, enlaces simbólicos y detección de programas que no cumplen)?
