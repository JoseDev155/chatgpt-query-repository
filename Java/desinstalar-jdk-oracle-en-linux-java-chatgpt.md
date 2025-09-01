¡Excelente que lo aclares! 🚀
La forma de **desinstalar JDK 8 de Oracle** depende de *cómo* lo instalaste en Debian 13. Te doy los tres escenarios típicos:

---

## 🔹 1. Si lo instalaste con `apt` (paquete `.deb`)

Ejemplo: descargaste el `.deb` de Oracle e hiciste `sudo dpkg -i jdk-8uXXX-linux-x64.deb`.

Para desinstalar:

```bash
# Busca el nombre exacto del paquete
dpkg -l | grep jdk
# Ejemplo de salida: oracle-java8-installer o jdk1.8-oracle

# Elimina el paquete
sudo apt remove --purge nombre_del_paquete

# Limpia dependencias innecesarias
sudo apt autoremove
```

---

## 🔹 2. Si lo instalaste manualmente (extrayendo un `.tar.gz`)

Ejemplo: descargaste `jdk-8uXXX-linux-x64.tar.gz`, lo descomprimiste en `/usr/lib/jvm/` o `/opt/`.

En ese caso **no está registrado en apt**, así que basta con **borrar la carpeta**:

```bash
sudo rm -rf /usr/lib/jvm/jdk1.8.0_XXX
# o en /opt/jdk1.8.0_XXX si ahí lo pusiste
```

Si lo tenías como predeterminado en `update-alternatives`, también conviene limpiarlo:

```bash
sudo update-alternatives --remove java /usr/lib/jvm/jdk1.8.0_XXX/bin/java
sudo update-alternatives --remove javac /usr/lib/jvm/jdk1.8.0_XXX/bin/javac
```

---

## 🔹 3. Si lo instalaste vía `update-alternatives` + carpeta manual

Algunos tutoriales recomiendan:

* Descomprimir el tarball en `/usr/lib/jvm/`
* Registrar manualmente con `update-alternatives`.

En ese caso debes:

1. **Quitar las alternativas**:

   ```bash
   sudo update-alternatives --remove java /usr/lib/jvm/jdk1.8.0_XXX/bin/java
   sudo update-alternatives --remove javac /usr/lib/jvm/jdk1.8.0_XXX/bin/javac
   ```

2. **Borrar la carpeta del JDK**:

   ```bash
   sudo rm -rf /usr/lib/jvm/jdk1.8.0_XXX
   ```

---

## 🔹 4. Revisar variables de entorno

Si en tu `~/.bashrc` o `~/.profile` habías agregado:

```bash
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_XXX
export PATH=$JAVA_HOME/bin:$PATH
```

Edita esos archivos y elimina esas líneas, o cámbialas para el nuevo JDK.

---

👉 Pregunta clave: ¿tú instalaste ese **JDK 8 de Oracle** con un `.tar.gz` (manual) o con un `.deb` (vía `dpkg/apt`)?
Así te indico exactamente los comandos que aplican a tu caso.
