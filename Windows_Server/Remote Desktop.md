# HABILITAR ESCRITORIO REMOTO WINDOWS SERVER
```powershell
# Habilitar RDP desde PowerShell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0   # habilita conexiones RDP

# Habilitar regla del firewall para Escritorio Remoto
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"   # abre puerto 3389 en firewall

# Verificar estado del servicio RDP
Get-Service TermService   # muestra estado del servicio Remote Desktop Services

# Iniciar y habilitar servicio automáticamente
Set-Service -Name TermService -StartupType Automatic   # inicio automático
Start-Service TermService   # inicia servicio

# Permitir autenticación NLA (recomendado)
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 1   # habilita NLA

# Agregar usuario al grupo de acceso RDP
Add-LocalGroupMember -Group "Remote Desktop Users" -Member "USUARIO"   # reemplazar USUARIO

# Verificar puerto escuchando
netstat -an | findstr 3389   # confirma escucha RDP

# Ver IP del servidor
ipconfig   # muestra IP local

# Probar conectividad desde otro equipo
mstsc   # abre cliente RDP en Windows
```

# HABILITAR DESDE CMD

```cmd
:: Habilitar RDP
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f   :: habilita RDP

:: Abrir firewall
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes   :: habilita reglas firewall

:: Iniciar servicio
sc config TermService start= auto   :: inicio automático
net start TermService   :: inicia servicio

:: Verificar puerto
netstat -ano | find ":3389"   :: verifica escucha
```

# VERIFICACIÓN

```powershell
Test-NetConnection localhost -Port 3389   # prueba puerto local
qwinsta   # muestra sesiones RDP
Get-NetFirewallRule -DisplayGroup "Remote Desktop"   # verifica firewall
```