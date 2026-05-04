
```sh
✅# **2. Verificar que las ZONAS DNS existen**
Get-Service DNS
Start-Service DNS
Get-DnsServerZone
```

###  Crear la zona inversa
```powershell
Add-DnsServerPrimaryZone -NetworkID "192.168.10.0/24" -ReplicationScope "Domain"

# Crear registros A
Add-DnsServerResourceRecordA -Name "pc1" -ZoneName "midominio.local" -IPv4Address "192.168.10.10"

# Crear Alias (CNAME)
Add-DnsServerResourceRecordCName -Name "archivos" -ZoneName "midominio.local" -HostNameAlias "pc2.midominio.local"

# Zona Inversa
# Crear Zona Inversa
Add-DnsServerPrimaryZone -NetworkID "10.0.2.0/24" -ReplicationScope Domain

#  Crear los PTR en la zona inversa
Add-DnsServerResourceRecordPtr -Name "10" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc1.midominio.local"
```

### Buscar DNS
```powershell
Resolve-DnsName -Name "tu-dominio.com" -Type ANY | Where-Object { $_.Name -like "*fbv-*" }

Get-DnsServerResourceRecord -ZoneName "ejemplo.com.do" |
Where-Object { $_.HostName -like "fbv-*" }
```

## Cambio de Dns (En caso de cambiar de dominio)
```c
Get-DnsServerZone

# Elimina el anterior
Remove-DnsServerZone -Name "cyber.local" -Force

# Añadir nuevo dominio
Add-DnsServerPrimaryZone -Name "pala.local" -ReplicationScope "Domain"
Add-DnsServerPrimaryZone -Name "_msdcs.pala.local" -ReplicationScope Forest

ipconfig /flushdns             # Borra la Cache
ipconfig /registerdns         # Actualizar Registros del nombre del equipo

Restart-Service DNS
Restart-Service Netlogon

# Verificar Netlogon (esto recrea los SRV)
net stop netlogon  
net start netlogon

# Verifica que el DC se registre correctamente
nltest /dsgetdc:pala.local
nltest /dsregdns

#Verificar registros críticos (esto es clave)
nslookup pala.local
nslookup -type=SRV _ldap._tcp.dc._msdcs.pala.local
```
