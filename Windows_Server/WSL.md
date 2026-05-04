
# WSL (GESTIÓN Y OPERACIÓN)------------
Explicación mínima: instalación, control de distros, backup, red e integración Windows↔WSL.
```bash
# VERIFICACIÓN
wsl --list                                         # listar distros instaladas
wsl --list --verbose                               # ver estado y versión (WSL1/2)
wsl -l -v                                          # ver distros y estado
wsl -l -v | findstr Running                        # filtrar distros activas
wsl --status                                       # estado general de WSL
wsl hostname -I                                    # ver IP dentro de WSL
wsl ip addr                                        # ver interfaces de red

# CONFIGURACIÓN
wsl --install                                      # instalar WSL (Ubuntu por defecto)
wsl --install -d Ubuntu-22.04                      # instalar distro específica
wsl --set-default <Distro>                         # definir distro por defecto
wsl --set-version <Distro> 2                       # cambiar a WSL2
wsl --set-version <Distro> 1                       # cambiar a WSL1
wsl --update                                       # actualizar kernel WSL

# CAMBIOS
wsl --terminate <Distro>                           # detener distro específica
wsl --shutdown                                     # apagar todas las distros
wsl --unregister <Distro>                          # eliminar distro completamente
wsl --mount <DiskPath>                             # montar disco físico
wsl --unmount <DiskPath>                           # desmontar disco

# BACKUP
wsl --export <Distro> C:\backup.tar                # exportar distro a backup
wsl --export Ubuntu backup.tar                     # backup rápido Ubuntu
wsl --import NuevaDistro C:\WSL\Nueva C:\backup.tar # importar distro desde backup
wsl --import-in-place NuevaDistro C:\backup.tar    # importar sin extraer

# EJECUCIÓN
wsl                                                # abrir distro por defecto
wsl -d <Distro>                                    # abrir distro específica
wsl -d <Distro> -u root                            # abrir como root
wsl --exec <comando>                               # ejecutar comando directo
wsl ls -la                                         # listar archivos desde Windows
wsl --cd ~                                         # abrir en home
wsl --cd /mnt/c                                    # abrir en disco C

# INTEGRACIÓN WINDOWS
explorer.exe .                                     # abrir carpeta actual en Explorer
notepad.exe archivo.txt                            # abrir archivo en Notepad
code .                                             # abrir en VS Code

# VALIDACIÓN
wsl -l -v                                          # confirmar estado de distros
wsl hostname -I                                    # validar conectividad interna

# DIAGNÓSTICO
dmesg | grep -i error                              # ver errores del kernel WSL
cat /etc/os-release                                # identificar distro activa
```

```bash
# ----------PERSISTENCIA DESMONTAJE /mnt/c EN WSL------------
Explicación mínima: WSL monta C: automáticamente; desactiva `automount`.

# VERIFICACIÓN
mount | grep /mnt/c                         # comprobar montaje automático
cat /etc/wsl.conf 2>/dev/null               # ver configuración actual

# CONFIGURACIÓN
sudo nano /etc/wsl.conf                     # crear/editar config de WSL
# agregar:
# [automount]
# enabled = false                           # desactiva montaje automático

# REINICIO
wsl --shutdown                             # apagar WSL desde Windows
# volver a abrir WSL                        # iniciar nuevamente

# VALIDACIÓN
mount | grep /mnt/c                         # no debe aparecer /mnt/c

# MONTAJE MANUAL (OPCIONAL)
sudo mount -t drvfs C: /mnt/c               # montar C manualmente cuando necesites

# DIAGNÓSTICO
cat /proc/filesystems | grep drvfs          # verificar soporte drvfs
dmesg | grep -i mount                      # revisar logs de montaje
```

```sh
# 1. Definir el comando y argumentos
$Action = New-ScheduledTaskAction -Execute "wsl.exe" -Argument "--distribution debian --exec dbus-launch true"

# 2. Configurar para que inicie al arrancar el sistema
$Trigger = New-ScheduledTaskTrigger -AtStartup

# 3. Configurar para que corra con privilegios de Administrador (evita la ventana negra)
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

# 4. Configurar ajustes (que funcione con batería y no se detenga)
$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries

# 5. Crear la tarea oficialmente
Register-ScheduledTask -TaskName "AutoStartWSL" -Action $Action -Trigger $Trigger -Principal $Principal -Settings $Settings

```


# HABILITAR PASSWORD AL ENTRAR A WSL---

```bash
# ----------CONFIGURAR USUARIO POR DEFECTO ROOT------------
wsl -u root                                   # entrar como root
nano /etc/wsl.conf                            # editar config

# ----------AGREGAR CONFIG------------
[boot]
command="exec /bin/login -f USUARIO"          # forzar login con password

# ----------ASIGNAR PASSWORD------------
passwd USUARIO                                # definir contraseña del usuario

# ----------REINICIAR WSL------------
exit                                          # salir
wsl --shutdown                                # reiniciar WSL
wsl --terminate

# ----------VERIFICACION------------
wsl                                           # debe pedir contraseña
```

# 
```bash
# MOVER VHDX WSL (SIN ROMPER INSTANCIA)

wsl --shutdown                                                # detener WSL completamente
Get-Service LxssManager | Stop-Service -Force # detener servicio WSL  
Get-Process *wsl* | Stop-Process -Force # matar procesos residuales  
Get-Process *vmmem* | Stop-Process -Force # liberar uso de memoria/VM  
  
# -------- VALIDAR BLOQUEO -----------  
handle.exe ext4.vhdx # identificar proceso (Sysinternals)  
# descargar si no existe:  
# https://learn.microsoft.com/sysinternals/downloads/handle

mkdir D:\WSL\Kali                                             # crear destino en nuevo disco
move "C:\Users\Randel\AppData\Local\Packages\KaliLinux.54290C8133FEE_ey8k8hqnwqnmg\LocalState\ext4.vhdx" "E:\DESCARGAS\MAQUINAS VIRTUALES\kali-wsl\ext4.vhdx"   # mover disco

# -------- REGISTRAR NUEVA UBICACION -----------
wsl --unregister Kali-linux                                   # eliminar registro (NO borra vhdx movido)
wsl --import Kali-linux D:\WSL\Kali D:\WSL\Kali\ext4.vhdx --version 2   # reimportar desde nueva ruta

# -------- VERIFICACION -----------
wsl -l -v                                                     # verificar estado e instalación
wsl -d Kali-linux                                             # iniciar distro
df -h                                                         # validar montaje dentro de WSL
```