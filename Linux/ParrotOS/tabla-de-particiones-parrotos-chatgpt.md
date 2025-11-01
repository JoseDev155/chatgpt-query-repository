Perfecto 👍 Veo que la tabla original está pensada para **Debian 13**, pero **ParrotOS** (basado en Debian) tiene algunos requisitos ligeramente distintos, especialmente en la parte de arranque UEFI.

En ParrotOS, si no se crea **`/boot/efi`**, el instalador puede fallar al generar la tabla de particiones, ya que necesita explícitamente ese punto de montaje (no solo “EFI”).
A continuación te muestro la **versión adaptada para ParrotOS**, manteniendo un esquema sólido y moderno:

| Partition     | Size   | File system          | Purpose                                                                       |
| ------------- | ------ | -------------------- | ----------------------------------------------------------------------------- |
| **/boot/efi** | 512 MB | **vfat (FAT32)**     | EFI System Partition (requerida para UEFI). Contiene el cargador de arranque. |
| **/boot**     | 2 GB   | **ext4**             | Partición del kernel y archivos de arranque.                                  |
| **swap**      | 33 GB  | **swap**             | Espacio de intercambio; tamaño recomendado = RAM + 1 (para hibernación).      |
| **/**         | 125 GB | **btrfs**            | Partición raíz del sistema. Ideal para snapshots y compresión.                |
| **/home**     | 1.8 TB | **xfs**              | Archivos de usuario y configuraciones personales.                             |
| **/var**      | 30 GB  | **xfs**              | Archivos variables: logs, bases de datos, cachés, etc.                        |
| **/tmp**      | 10 GB  | **ext4** o **tmpfs** | Archivos temporales del sistema.                                              |

### 🔍 Diferencias clave respecto a Debian 13:

1. Se reemplaza **“EFI”** por **“/boot/efi”**, que es el punto de montaje correcto en ParrotOS (y la mayoría de distribuciones modernas).
2. El sistema de archivos EFI debe ser **vfat (FAT32)**.
3. ParrotOS puede usar **btrfs** para `/` sin problema, pero también es común usar **ext4** si prefieres simplicidad o estabilidad.
4. Mantiene la estructura multi-partición avanzada, ideal si piensas experimentar o hacer pruebas de seguridad (por ejemplo, auditorías con Parrot).
