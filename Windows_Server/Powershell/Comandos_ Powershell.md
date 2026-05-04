# PERFIL / ALIAS / FUNCIONES
```bash
notepad $PROFILE                                      # abrir perfil PowerShell
New-Item -ItemType File -Path $PROFILE -Force          # crear perfil si no existe
. $PROFILE                                             # recargar perfil

Set-Alias pingloop "C:\scripts\pingloop.bat"           # alias personalizado
Set-Alias ll Get-ChildItem                             # alias útil
Set-Alias sshc ssh                                     # alias ssh corto

function adminps { Start-Process powershell -Verb RunAs }   # abrir como admin
function mikro { ssh -o MACs=hmac-sha1 -o Ciphers=aes128-cbc -o KexAlgorithms=+diffie-hellman-group1-sha1 usuario@10.0.0.50 }  # ssh legacy
function findhist($kw) { Select-String -Path (Get-PSReadLineOption).HistorySavePath -Pattern $kw }  # buscar historial
```
# SESIÓN / SISTEMA
```bash
saps powershell -Verb runas                     # abrir PowerShell admin (alias)
Start-Process powershell -Verb RunAs            # forma completa
whoami                                          # usuario actual
hostname                                        # nombre equipo
Get-ComputerInfo                                # info sistema
gcim Win32_OperatingSystem                      # info OS (alias)
Get-CimInstance Win32_OperatingSystem           # info OS completa
```
# PROCESOS / SERVICIOS
```bash
gps                                             # listar procesos
gps chrome                                      # filtrar proceso
Stop-Process -Name chrome                       # detener proceso
kill -Name chrome                               # alias kill
Start-Process notepad                           # iniciar app

gsv                                             # listar servicios
gsv wuauserv                                    # servicio específico
Start-Service wuauserv                          # iniciar
Stop-Service wuauserv                           # detener
Restart-Service wuauserv                        # reiniciar
Set-Service wuauserv -StartupType Automatic     # auto inicio
```
# RED
```bash
ipconfig                                        # IP básica
Get-NetIPAddress                                # IPs
Get-NetIPConfiguration                          # config completa
tnc 8.8.8.8                                     # ping moderno

Get-NetAdapter                                  # interfaces
Enable-NetAdapter -Name "Ethernet"              # habilitar NIC
Disable-NetAdapter -Name "Ethernet"             # deshabilitar NIC

Get-NetTCPConnection                            # conexiones TCP
netstat -ano                                    # conexiones + PID
```
# USUARIOS / GRUPOS
```bash
net user                                        # listar usuarios
net user user1 pass123 /add                     # crear usuario
net user user1 /delete                          # eliminar usuario

net localgroup Administrators                   # ver admins
net localgroup Administrators user1 /add        # agregar admin

Get-LocalUser                                   # usuarios PS
New-LocalUser user1                             # crear
Remove-LocalUser user1                          # eliminar
```
# ARCHIVOS / DISCOS
```bash
Get-PSDrive                                     # discos lógicos
Get-Disk                                        # discos físicos
Get-Volume                                      # volúmenes

ls                                              # listar archivos
cd C:\                                          # cambiar ruta

cp file.txt C:\backup\                          # copiar
mv file.txt new.txt                             # mover
rm file.txt                                     # eliminar

mkdir test                                      # crear carpeta
```
# LOGS / EVENTOS
```bash
Get-WinEvent -LogName System                    # logs sistema
Get-WinEvent -MaxEvents 50                      # últimos eventos
```
# FIREWALL
```bash
Get-NetFirewallRule                             # reglas
Enable-NetFirewallRule -DisplayName "Remote Desktop"   # habilitar regla
Disable-NetFirewallRule -DisplayName "Remote Desktop"  # deshabilitar
```
# SISTEMA / HARDWARE
```bash
Get-Process | measure                           # total procesos
Get-Service | where {$_.Status -eq "Running"}   # servicios activos

gwmi Win32_ComputerSystem                       # hardware (alias)
Get-WmiObject Win32_ComputerSystem              # completo
```
# SOFTWARE / UPDATES
```bash
wmic product get name                           # programas (legacy)
Get-Package                                     # paquetes

Get-WindowsUpdate                               # listar updates
Install-WindowsUpdate                           # instalar updates
```
# APAGADO / REMOTO
```bash
shutdown /s /t 0                                # apagar
shutdown /r /t 0                                # reiniciar

Restart-Computer                                # reiniciar PS
Stop-Computer                                   # apagar PS

Enter-PSSession -ComputerName PC1               # sesión remota
Invoke-Command -ComputerName PC1 -ScriptBlock { hostname }  # ejecutar remoto
```
# SCRIPTS / HELP
```bash
Set-ExecutionPolicy RemoteSigned                # permitir scripts
.\script.ps1                                    # ejecutar script

Get-Help Get-Process                            # ayuda
man Get-Process                                 # alias help
Get-Command                                     # listar comandos
```
# HISTORIAL / AUTOCOMPLETADO
```bash
Get-PSReadLineOption | Select HistorySavePath   # ruta historial
Get-Content (Get-PSReadLineOption).HistorySavePath   # ver historial
Get-History                                     # sesión actual

Set-PSReadLineOption -HistorySaveStyle SaveIncrementally   # guardar automático
Set-PSReadLineOption -PredictionSource HistoryAndPlugin    # autocompletado inteligente
Set-PSReadLineOption -PredictionViewStyle ListView         # vista lista
```
# ALIAS / INSPECCIÓN
```bash
Get-Alias                                       # listar alias
gal                                             # alias corto
Get-Alias saps                                  # ver alias específico
Get-Alias sp                                    # ver comando real
```
# AUTOCOMPLETADO HISTORIAL (PSReadLine)
```bash
Set-PSReadLineOption -HistorySaveStyle SaveIncrementally   # guarda comandos en tiempo real
Set-PSReadLineOption -MaximumHistoryCount 10000            # ampliar historial
Set-PSReadLineOption -PredictionSource History             # usar historial como predicción
Set-PSReadLineOption -PredictionViewStyle ListView         # vista tipo lista
```
# BÚSQUEDA RÁPIDA (TIPO CTRL+R MEJORADO)
```bash
Set-PSReadLineKeyHandler -Key Ctrl+r -Function ReverseSearchHistory   # buscar hacia atrás
Set-PSReadLineKeyHandler -Key Ctrl+s -Function ForwardSearchHistory   # buscar hacia adelante
```
# AUTOCOMPLETADO INTELIGENTE
```bash
Set-PSReadLineOption -PredictionSource HistoryAndPlugin   # historial + plugins
Set-PSReadLineOption -ShowToolTips                        # mostrar ayuda en sugerencias
```
# EJECUTAR COMANDOS FRECUENTES RÁPIDO
```bash
Set-Alias ll Get-ChildItem        # alias común
Set-Alias sshc ssh                # alias corto SSH
Set-Alias gs Get-Service          # alias servicios
```
# CREAR FUNCIONES REUTILIZABLES
```bash
function ssh-wg { ssh -p 4118 randel.melenciano@172.26.16.1 }   # acceso rápido WatchGuard
function findhist($kw) { Select-String -Path (Get-PSReadLineOption).HistorySavePath -Pattern $kw }  # buscar en historial
```
# CARGA AUTOMÁTICA (PERSISTENCIA)
```bash
notepad $PROFILE                # abrir perfil
# pegar todas las configuraciones anteriores y guardar
```
# LIMPIAR / MANTENER HISTORIAL
```bash
Clear-History                   # limpiar sesión actual
Remove-Item (Get-PSReadLineOption).HistorySavePath   # borrar historial persistente
```
# VERIFICACIÓN
```bash
Get-PSReadLineOption | Select PredictionSource,PredictionViewStyle,MaximumHistoryCount   # validar config
```