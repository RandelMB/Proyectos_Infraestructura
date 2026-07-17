# FIREWALL WINDOWS POWERSHELL
```powershell
# abrir PowerShell como administrador
Start-Process powershell -Verb runAs                              # abrir PowerShell elevado

# ver configuracion IP
Get-NetIPConfiguration                                            # mostrar IP, gateway, DNS y adaptadores
ipconfig /all                                                     # ver configuracion IP detallada
Get-NetIPAddress                                                  # listar direcciones IPv4 e IPv6
Get-DnsClientServerAddress                                        # mostrar servidores DNS configurados

# ver estado del firewall
Get-NetFirewallProfile                                            # estado de perfiles Domain, Private y Public
Get-NetFirewallRule                                               # listar reglas configuradas
Get-NetFirewallSetting                                            # mostrar configuracion general del firewall
netsh advfirewall show allprofiles                                # mostrar estado completo del firewall

# activar firewall en todos los perfiles
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True   # habilitar firewall globalmente
netsh advfirewall set allprofiles state on                        # activar firewall mediante netsh

# desactivar firewall en todos los perfiles
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False  # deshabilitar firewall
netsh advfirewall set allprofiles state off                       # desactivar firewall mediante netsh

# bloquear ping entrante ICMPv4
New-NetFirewallRule -DisplayName "Bloquear Ping" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Block   # bloquear respuestas ping IPv4
netsh advfirewall firewall add rule name="Bloquear Ping" protocol=icmpv4:8,any dir=in action=block   # bloquear ping usando netsh

# permitir ping entrante ICMPv4
New-NetFirewallRule -DisplayName "Permitir Ping" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow   # permitir ping IPv4
netsh advfirewall firewall add rule name="Permitir Ping" protocol=icmpv4:8,any dir=in action=allow   # permitir ping usando netsh

# abrir puerto especifico
New-NetFirewallRule -DisplayName "Abrir Puerto 3389" -Direction Inbound -Protocol TCP -LocalPort 3389 -Action Allow   # permitir RDP
netsh advfirewall firewall add rule name="RDP" dir=in action=allow protocol=TCP localport=3389   # abrir puerto RDP

# bloquear puerto especifico
New-NetFirewallRule -DisplayName "Bloquear Puerto 445" -Direction Inbound -Protocol TCP -LocalPort 445 -Action Block   # bloquear SMB
netsh advfirewall firewall add rule name="Bloquear SMB" dir=in action=block protocol=TCP localport=445   # bloquear SMB

# eliminar regla firewall
Remove-NetFirewallRule -DisplayName "Bloquear Ping"               # eliminar regla PowerShell
netsh advfirewall firewall delete rule name="Bloquear Ping"       # eliminar regla netsh

# exportar configuracion firewall
netsh advfirewall export "C:\firewall-backup.wfw"                 # exportar reglas firewall

# importar configuracion firewall
netsh advfirewall import "C:\firewall-backup.wfw"                 # restaurar reglas firewall

# restaurar configuracion por defecto
netsh advfirewall reset                                           # resetear firewall a valores predeterminados

# verificar reglas activas
Get-NetFirewallRule | Where-Object Enabled -eq True               # listar reglas activas
Get-NetFirewallRule | Select DisplayName, Enabled, Direction      # mostrar reglas resumidas

# probar conectividad
Test-NetConnection 8.8.8.8                                       # probar conectividad IP
Test-NetConnection google.com -Port 443                           # probar acceso HTTPS
ping 8.8.8.8                                                      # verificar ICMP
```


# MODIFICAR REGLAS FIREWALL WINDOWS
```powershell
# ver reglas existentes
Get-NetFirewallRule                                               # listar todas las reglas
Get-NetFirewallRule | Select DisplayName, Enabled, Direction      # resumen de reglas
Get-NetFirewallRule -DisplayName "*Ping*"                         # buscar regla especifica
netsh advfirewall firewall show rule name=all                     # mostrar reglas mediante netsh

# deshabilitar una regla
Disable-NetFirewallRule -DisplayName "Bloquear Ping"              # desactivar regla
netsh advfirewall firewall set rule name="Bloquear Ping" new enable=no   # desactivar con netsh

# habilitar una regla
Enable-NetFirewallRule -DisplayName "Bloquear Ping"               # activar regla
netsh advfirewall firewall set rule name="Bloquear Ping" new enable=yes  # activar con netsh

# cambiar accion de una regla
Set-NetFirewallRule -DisplayName "Bloquear Ping" -Action Allow    # cambiar de Block a Allow
Set-NetFirewallRule -DisplayName "Permitir Ping" -Action Block    # cambiar de Allow a Block

# cambiar puerto de una regla
Set-NetFirewallRule -DisplayName "RDP" -LocalPort 3390            # modificar puerto
netsh advfirewall firewall set rule name="RDP" new localport=3390   # cambiar puerto con netsh

# cambiar protocolo
Set-NetFirewallRule -DisplayName "RDP" -Protocol UDP              # cambiar protocolo TCP/UDP
netsh advfirewall firewall set rule name="RDP" new protocol=UDP   # modificar protocolo

# cambiar direccion trafico
Set-NetFirewallRule -DisplayName "RDP" -Direction Outbound        # cambiar entrada/salida
netsh advfirewall firewall set rule name="RDP" new dir=out        # modificar direccion

# cambiar perfiles
Set-NetFirewallRule -DisplayName "RDP" -Profile Public            # aplicar solo perfil Public
Set-NetFirewallRule -DisplayName "RDP" -Profile Domain,Private    # aplicar multiples perfiles

# cambiar IP remota permitida
Set-NetFirewallRule -DisplayName "RDP" -RemoteAddress 192.168.1.0/24   # limitar acceso por IP
netsh advfirewall firewall set rule name="RDP" new remoteip=192.168.1.0/24   # limitar IP con netsh

# renombrar regla
Set-NetFirewallRule -DisplayName "RDP" -NewDisplayName "RDP Seguro"   # cambiar nombre regla

# verificar cambios
Get-NetFirewallRule -DisplayName "*RDP*" | Format-List            # mostrar configuracion completa
netsh advfirewall firewall show rule name="RDP Seguro"            # verificar con netsh
```
# LISTAR REGLAS FIREWALL RESUMIDAS
```powershell
# listado corto de reglas
Get-NetFirewallRule | Select DisplayName, Enabled, Direction, Action   # resumen basico

# listado compacto en tabla
Get-NetFirewallRule | Format-Table DisplayName, Enabled, Direction, Action -AutoSize   # tabla compacta

# mostrar solo reglas habilitadas
Get-NetFirewallRule | Where-Object Enabled -eq True | Select DisplayName, Direction, Action   # reglas activas

# mostrar reglas bloqueadas
Get-NetFirewallRule | Where-Object Action -eq Block | Select DisplayName, Direction   # reglas bloqueadas

# mostrar reglas permitidas
Get-NetFirewallRule | Where-Object Action -eq Allow | Select DisplayName, Direction   # reglas permitidas

# exportar listado resumido
Get-NetFirewallRule | Select DisplayName, Enabled, Direction, Action | Out-File C:\firewall-reglas.txt   # guardar listado

# listar usando netsh
netsh advfirewall firewall show rule name=all                       # mostrar todas las reglas
```