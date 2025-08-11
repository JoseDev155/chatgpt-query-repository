No, no te creará conflictos 🙂.
Montar en `/mnt` es **solo un paso temporal** que se hace durante la instalación o mantenimiento desde un entorno *live* (como el instalador de Debian o un live USB).

Te explico el flujo para que lo veas claro:

---

### 1️⃣ Durante la instalación o mantenimiento

* Estás en un entorno *live*, que tiene su **propio** `/` independiente del que vas a instalar.
* Montas la partición raíz de tu futuro sistema (la que es Btrfs) en `/mnt` para poder manipularla:

```bash
mount -o subvol=@rootfs /dev/sdXY /mnt
```

* Una vez ahí, puedes renombrar `@rootfs` → `@`, reorganizar subvolúmenes, etc.
* Mientras trabajas aquí, si haces:

```bash
cd /etc/apt/sources.list
```

**estarías entrando al `/etc` del sistema live**, no al del sistema que estás montando.
Para entrar al `/etc` de tu instalación, tendrías que hacer:

```bash
cd /mnt/etc/apt/sources.list
```

---

### 2️⃣ Después de reiniciar al sistema instalado

* El subvolumen `@` se monta directamente en `/` gracias a la entrada en `/etc/fstab`.
* Ahora tu sesión **sí** está dentro de tu instalación, y cualquier ruta como:

```bash
cd /etc/apt/sources.list
```

irá directamente al archivo de **tu sistema real**.

* `/mnt` volverá a ser un directorio vacío o usado para montajes temporales.

---

✅ **Conclusión:**
No es que cambie la forma de acceder a las rutas permanentemente; el montaje en `/mnt` es sólo para manipular el sistema antes de arrancarlo. Una vez que arrancas en el sistema instalado, todo se comporta como siempre, y `/etc/apt/sources.list` será accesible sin prefijos adicionales.

---

Si quieres, te puedo dibujar un **esquema paso a paso** de cómo se ve `/` y `/mnt` en ambas fases para que visualices mejor el cambio.
