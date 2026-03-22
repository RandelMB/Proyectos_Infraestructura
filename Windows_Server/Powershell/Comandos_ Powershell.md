
```sh
# EJECUCIÓN / SESIÓN
saps powershell -Verb runas             # abre PowerShell como admin (abreviatura)
Start-Process powershell -Verb RunAs    # forma completa

whoami                                  # usuario actual
hostname                                # nombre del equipo
Get-ComputerInfo                        # info completa del sistema
gcim Win32_OperatingSystem              # info OS (abreviado CIM)
Get-CimInstance Win32_OperatingSystem   # forma completa
```

```sh
# USANDO saps (alias nativo)
saps powershell -Verb runas

# CREAR ALIAS  PARA apps y 
Set-Alias pingloop "C:\scripts\pingloop.bat"

# Agregar Alias Permanete (`\Users\[tu_usuario]\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`)
notepad $PROFILE                                    # agregalo en notepad

# Si te da error (no existe), créalo:               
New-Item -ItemType File -Path $PROFILE -Force  
notepad $PROFILE
. $PROFILE                                          # Guardalo

# Acceso rapido a comandos
function adminps {
    Start-Process powershell -Verb RunAs
}

function mikro {
    ssh -o MACs=hmac-sha1 -o Ciphers=aes128-cbc -o KexAlgorithms=+diffie-hellman-group1-sha1 usuario@10.0.0.50
}
```

```sh
# PROCESOS
gps                                     # listar procesos
Get-Process                             # forma completa
gps chrome                              # filtrar por nombre
Stop-Process -Name chrome               # matar proceso
kill -Name chrome                       # abreviatura
Start-Process notepad                   # iniciar proceso

# SERVICIOS
gsv                                     # listar servicios
Get-Service                             # completo
gsv wuauserv                            # servicio específico
Start-Service wuauserv                  # iniciar servicio
Stop-Service wuauserv                   # detener servicio
Restart-Service wuauserv                # reiniciar servicio
Set-Service wuauserv -StartupType Automatic  # arranque automático


# RED
ipconfig                                # config IP (CMD clásico)
Get-NetIPAddress                        # IPs en PowerShell
Get-NetIPConfiguration                  # config completa
Test-Connection 8.8.8.8                 # ping
tnc 8.8.8.8                             # abreviatura de Test-Connection

Get-NetAdapter                          # interfaces
Enable-NetAdapter -Name "Ethernet"      # habilitar NIC
Disable-NetAdapter -Name "Ethernet"     # deshabilitar NIC

Get-NetTCPConnection                    # conexiones TCP
netstat -ano                            # conexiones + PID

# ================================
# USUARIOS Y GRUPOS
# ================================
net user                                # listar usuarios
net user user1 pass123 /add             # crear usuario
net user user1 /delete                  # borrar usuario

net localgroup Administrators           # ver admins
net localgroup Administrators user1 /add  # agregar a admin

Get-LocalUser                           # listar usuarios (PS)
New-LocalUser user1                     # crear usuario
Remove-LocalUser user1                  # eliminar usuario

# ================================
# DISCOS Y ARCHIVOS
# ================================
Get-PSDrive                             # discos
Get-Disk                                # discos físicos
Get-Volume                              # volúmenes

ls                                      # listar archivos (alias)
Get-ChildItem                           # completo
cd C:\                                  # cambiar directorio
Set-Location C:\                        # completo

cp file.txt C:\backup\                  # copiar (alias)
Copy-Item file.txt C:\backup\           # completo
mv file.txt new.txt                     # mover/renombrar
Move-Item file.txt new.txt              # completo
rm file.txt                             # eliminar
Remove-Item file.txt                    # completo

mkdir test                              # crear carpeta
New-Item -ItemType Directory test       # completo

# ================================
# PERMISOS
# ================================
icacls C:\datos                         # ver permisos
icacls C:\datos /grant user1:F          # dar permisos

# ================================
# EVENTOS Y LOGS
# ================================
Get-EventLog -LogName System            # logs clásicos
Get-WinEvent -LogName System            # logs modernos
Get-WinEvent -MaxEvents 50              # últimos eventos

# ================================
# ACTUALIZACIONES
# ================================
Get-WindowsUpdate                       # ver updates (requiere módulo)
Install-WindowsUpdate                   # instalar updates

# ================================
# FIREWALL
# ================================
Get-NetFirewallRule                     # reglas
Enable-NetFirewallRule -DisplayName "Remote Desktop"
Disable-NetFirewallRule -DisplayName "Remote Desktop"

# ================================
# SISTEMA / HARDWARE
# ================================
Get-Process | measure                   # conteo procesos
Get-Service | where {$_.Status -eq "Running"}  # servicios activos

gwmi Win32_ComputerSystem              # info hardware (alias)
Get-WmiObject Win32_ComputerSystem     # completo

# ================================
# SOFTWARE
# ================================
wmic product get name                  # programas instalados (legacy)
Get-Package                            # paquetes (PS moderno)

# APAGADO / REINICIO
shutdown /s /t 0                       # apagar
shutdown /r /t 0                       # reiniciar

Restart-Computer                       # reiniciar (PS)
Stop-Computer                          # apagar (PS)

# REMOTO
Enter-PSSession -ComputerName PC1      # sesión remota
Invoke-Command -ComputerName PC1 -ScriptBlock { hostname }


# EJECUCIÓN DE SCRIPTS
Set-ExecutionPolicy RemoteSigned       # permitir scripts
.\script.ps1                           # ejecutar script

# ================================
# HELP
# ================================
Get-Help Get-Process                   # ayuda
man Get-Process                        # alias
Get-Command                            # listar comandos
```

```sh
# VER ALIAS EXISTENTES
Get-Alias
gal

# =========================================
# VER COMANDO REAL DETRÁS DEL ALIAS
# =========================================
Get-Alias saps
Get-Alias sp
```