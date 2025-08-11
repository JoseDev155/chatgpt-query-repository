# Esquema paso a paso

Perfecto — aquí tienes un **esquema visual** y claro de lo que pasa en cada etapa (con comandos ejemplo). Mantendré las cosas concisas para que sea fácil de seguir.

---

## Estado inicial (antes de tocar nada)

Particiones físicas tal como indicaste:

```
/dev/sda1   ─ /boot   ─ 512 MB
/dev/sda2   ─ swap    ─ 17 GB
/dev/sda3   ─ BTRFS   ─ 50 GB   (contiene subvolumen @rootfs)
/dev/sda4   ─ XFS     ─ 50 GB   (montado como /home)
```

Estructura lógica dentro de la partición Btrfs (antes de renombrar):

```
BTRFS ( /dev/sda3 )
└─ @rootfs/        ← contiene /etc, /usr, /var, ...
```

`/home` está en la partición XFS separada:

```
XFS ( /dev/sda4 )
└─ (datos de /home)
```

---

## Paso 1 — Estamos en el live (o instalador): montamos la Btrfs en `/mnt`

Comando típico:

```bash
# ejemplo (ajusta /dev/sda3 según corresponda)
mount -o subvol=@rootfs /dev/sda3 /mnt
```

Estado visual mientras trabajas desde el live:

```
LIVE-ENVIRONMENT (su propio /)
├─ /bin, /etc (del live)
└─ /mnt  (montaje de la BTRFS del sistema que vas a instalar)
   └─ @rootfs/
       ├─ etc/
       ├─ usr/
       └─ var/
```

Nota: si ejecutas `cd /etc` estás entrando al `/etc` del live. Para ver el `/etc` del sistema que instalas: `cd /mnt/etc`.

---

## Paso 2 — Renombrar `@rootfs` → `@` (reorganizar subvolúmenes)

**Forma segura recomendada (crear snapshot / subvol nuevo y borrar el viejo):**

```bash
# estando con la btrfs montada en /mnt
btrfs subvolume snapshot /mnt/@rootfs /mnt/@       # crea @ como copia (writable)
btrfs subvolume list /mnt                          # comprobar que @ existe
# si todo OK y quieres eliminar el antiguo:
btrfs subvolume delete /mnt/@rootfs
```

(Alternativa más simple pero menos "prudente": `mv /mnt/@rootfs /mnt/@` — funciona en muchos casos, pero la opción de snapshot es más segura porque mantiene el original hasta que verificas.)

Después del cambio (dentro de la partición Btrfs):

```
BTRFS ( /dev/sda3 )
├─ @/         ← ahora será el subvolumen que queremos montar como "/"
└─ (otros subvols opcionales: @home, @snapshots, @var, ...)
```

Si quieres mover `/home` dentro de Btrfs (opcional):

```bash
btrfs subvolume create /mnt/@home
# mover o sincronizar contenido de /mnt/home hacia /mnt/@home
mv /mnt/home/* /mnt/@home/
```

(en tu caso `/home` ya está en XFS; moverlo es opcional).

---

## Paso 3 — Ajustar `/etc/fstab` para el arranque final

Ejemplo de líneas (usar UUIDs reales con `blkid` en vez de `/dev/sdX`):

```
# /etc/fstab (ejemplo)
UUID=XXXX-XXXX  /      btrfs  defaults,subvol=@,compress=zstd,space_cache=v2 0 0
UUID=YYYY-YYYY  /boot  vfat   defaults                                   0 1
UUID=ZZZZ-ZZZZ  /home  xfs    defaults                                   0 2
UUID=WWWW-WWWW  none   swap   sw                                         0 0
```

Con esto, al arrancar por primera vez desde el disco, el kernel/mount montará el subvolumen `@` directamente en `/`.

---

## Paso 4 — Estado final (tras reiniciar en el sistema instalado)

Sistema montado en vivo (ya no hay `/mnt` con el root ahí, a menos que lo uses para otros montajes):

```
Sistema arrancado
/
├─ etc/    (dentro del subvolumen @ en la BTRFS)
├─ usr/    (idem)
└─ home/   (montado desde /dev/sda4 XFS)
```

Y la partición BTRFS contiene ahora un layout de subvolúmenes:

```
BTRFS ( /dev/sda3 )
├─ @          ← montado como /
├─ @home?     ← si creaste y usas /home en btrfs
└─ @snapshots ← si implementas snapshots
```

---

## Recordatorios útiles / comandos para trabajar antes de reiniciar

* Para editar archivos del sistema instalado desde el live (p. ej. `/etc/apt/sources.list`):

  ```bash
  nano /mnt/etc/apt/sources.list
  ```
* Para "entrar" al sistema instalado (chroot) y trabajar como si estuvieras dentro:

  ```bash
  mount --bind /dev  /mnt/dev
  mount --bind /proc /mnt/proc
  mount --bind /sys  /mnt/sys
  chroot /mnt /bin/bash
  ```
* Comprueba subvolúmenes:

  ```bash
  btrfs subvolume list /mnt
  ```

---

Si quieres, te dibujo ahora **un diagrama específico con tus nombres de dispositivos** (por ejemplo `/dev/sda3`) y te doy los comandos exactos listos para copiar/pegar — o te genero el `/etc/fstab` con UUIDs ficticios para que veas el formato. ¿Te interesa eso?
