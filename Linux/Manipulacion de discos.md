# GESTION BASICA DE DISCOS (VARIANTES)
```bash
lsblk                                       # lista básica de dispositivos
lsblk -f                                    # muestra FS, UUID y montajes
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,FSTYPE   # columnas personalizadas
lsblk -p                                    # muestra rutas completas (/dev/...)

fdisk -l                                    # lista particiones MBR/GPT
cfdisk /dev/sdX                             # interfaz interactiva (ncurses)
sfdisk -l /dev/sdX                          # listar con sfdisk

blkid                                       # UUID y tipo FS
blkid /dev/sdX1                             # info específica

df -h                                       # uso de discos legible
df -Th                                      # incluye tipo FS
df -i                                       # uso de inodos

du -sh /ruta                                # tamaño total
du -h --max-depth=1 /ruta                   # detalle por carpeta
du -ah /ruta | sort -rh | head              # top archivos pesados
```
# PARTICIONADO
```bash
fdisk /dev/sdX                              # particionar MBR
fdisk -l /dev/sdX                           # listar solo un disco

parted /dev/sdX print                       # ver tabla GPT/MBR
parted /dev/sdX mklabel gpt                 # crear GPT
parted /dev/sdX mkpart primary ext4 1MiB 100% # crear partición

partprobe /dev/sdX                          # recargar tabla
kpartx -a /dev/sdX                          # mapear particiones

wipefs -a /dev/sdX                          # borrar firmas FS
wipefs -n /dev/sdX                          # simular borrado

dd if=/dev/zero of=/dev/sdX bs=1M status=progress # limpiar disco completo
dd if=/dev/urandom of=/dev/sdX bs=1M count=100    # overwrite aleatorio

sgdisk -Z /dev/sdX                          # borrar GPT/MBR
sgdisk -o /dev/sdX                          # crear nueva GPT
```
# FORMATEO 
```bash
mkfs.ext4 /dev/sdX1                         # EXT4 básico
mkfs.ext4 -L DATA /dev/sdX1                 # con etiqueta
mkfs.ext4 -F /dev/sdX1                      # forzar formateo

mkfs.xfs /dev/sdX1                          # XFS
mkfs.xfs -f /dev/sdX1                       # forzar XFS

mkfs.vfat -F 32 /dev/sdX1                   # FAT32
mkfs.ntfs -f /dev/sdX1                      # NTFS

mkswap /dev/sdX2                            # crear swap
mkswap -L SWAP /dev/sdX2                    # swap con label
```
# MONTAJE
```bash
mount /dev/sdX1 /mnt                        # montaje básico
mount -t ext4 /dev/sdX1 /mnt                # especificar FS
mount -o ro /dev/sdX1 /mnt                  # solo lectura
mount -o rw,noatime /dev/sdX1 /mnt          # optimizado

umount /mnt                                 # desmontar por ruta
umount /dev/sdX1                            # desmontar por device
umount -l /mnt                              # lazy unmount
umount -f /mnt                              # forzado

mount -a                                    # montar fstab
findmnt                                     # árbol de montajes
findmnt -S /dev/sdX1                        # buscar montaje específico
```
# FSTAB 
```bash
nano /etc/fstab                             # editar
blkid | grep sdX1                           # obtener UUID

mount -o remount /mnt                       # recargar opciones
systemctl daemon-reload                     # recargar systemd
systemctl daemon-reexec                     # reinicializar systemd

mount -a -v                                 # validar fstab verbose
```

# LVM (VARIANTES)

```bash
pvcreate /dev/sdX1                          # crear PV
pvscan                                      # escaneo PV
pvs                                         # resumen PV

vgcreate vg0 /dev/sdX1                      # crear VG
vgextend vg0 /dev/sdX2                      # extender VG
vgreduce vg0 /dev/sdX2                      # reducir VG
vgs                                         # resumen VG

lvcreate -n lv0 -L 10G vg0                  # crear LV tamaño fijo
lvcreate -n lv0 -l 100%FREE vg0             # usar todo el espacio
lvdisplay                                   # info LV
lvs                                         # resumen LV

lvextend -l +100%FREE /dev/vg0/lv0          # expandir todo libre
lvreduce -r -L 5G /dev/vg0/lv0              # reducir y FS
lvremove -f /dev/vg0/lv0                    # eliminar LV
```

# REDIMENSION (VARIANTES)

```bash
resize2fs /dev/vg0/lv0                      # expandir EXT4
resize2fs /dev/vg0/lv0 5G                   # tamaño específico

xfs_growfs /mnt                             # crecer XFS montado

e2fsck -f /dev/vg0/lv0                      # chequeo EXT4
e2fsck -p /dev/vg0/lv0                      # automático

tune2fs -l /dev/sdX1                        # info EXT FS
```

# RAID MDADM (VARIANTES)

```bash
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdX1 /dev/sdX2 # RAID1
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdX1 /dev/sdX2 /dev/sdX3 # RAID5

mdadm --detail /dev/md0                     # estado
mdadm --examine /dev/sdX1                   # metadata disco

mdadm --add /dev/md0 /dev/sdX4              # agregar disco
mdadm --fail /dev/md0 /dev/sdX2             # marcar fallo
mdadm --remove /dev/md0 /dev/sdX2           # remover

mdadm --stop /dev/md0                       # detener
mdadm --assemble --scan                     # ensamblar automático
```

# CIFRADO LUKS (VARIANTES)

```bash
cryptsetup luksFormat /dev/sdX1             # cifrar
cryptsetup luksOpen /dev/sdX1 secure_disk   # abrir
cryptsetup luksClose secure_disk            # cerrar

cryptsetup luksDump /dev/sdX1               # info cabecera
cryptsetup luksAddKey /dev/sdX1             # añadir clave
cryptsetup luksRemoveKey /dev/sdX1          # eliminar clave
```

# SWAP (VARIANTES)

```bash
swapon /dev/sdX2                            # activar
swapon -a                                   # activar desde fstab
swapoff /dev/sdX2                           # desactivar

swapon --show                               # ver swap activo
cat /proc/swaps                             # detalle kernel
free -h                                     # uso RAM/SWAP
```

# VERIFICACION FINAL

```bash
lsblk -f                                    # estructura completa
mount | column -t                           # montajes formateados
findmnt -r                                  # vista limpia

vgs && lvs && pvs                           # estado LVM
mdadm --detail --scan                       # estado RAID
blkid                                       # UUID/FS finales
```
# CAMBIAR PUNTO DE MONTAJE (/mnt/cloud-storage → /mnt)
```bash
umount /mnt/cloud-storage                     # desmontar (NO elimina datos)
mount /dev/sdb1 /mnt                          # montar en nuevo punto

# persistente en fstab (recomendado usar UUID)
blkid /dev/sdb1                               # obtener UUID
nano /etc/fstab                               # editar entrada

# EJEMPLO fstab (reemplazar ruta antigua)
# UUID=XXXX-XXXX  /mnt  ext4  defaults  0  2

mount -a                                      # aplicar cambios
```
# LIMPIEZA Y AJUSTES
```bash
rmdir /mnt/cloud-storage                      # eliminar carpeta si vacía
mkdir -p /mnt                                 # asegurar punto de montaje
```
# VERIFICACION
```bash
df -h | grep sdb1                             # validar nuevo montaje
lsblk -f                                      # verificar punto de montaje
findmnt /mnt                                  # confirmar montaje activo
```
