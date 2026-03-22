


```sh
# 1. Renombrar el dominio (Domain Rename)

### Herramientas usadas
- rendom    
- gpfixup    
- netdom    

# Primero visualiza el dominio actual y el NetBIOS
(Get-ADDomain).DNSRoot
(Get-ADDomain).NetBIOSName

# 1. Generar configuración del bosque (Domainlist.xml) (Debes Reemplazar Dominio y NetBIOS)  
rendom /list

# 3. Subir la nueva configuración    
rendom /upload

# 4. Preparar los controladores de dominio
rendom /prepare

# 5. Ejecutar el cambio /(Se va a reiniciar)
rendom /execute

# 6. Corregir las GPO ( Poner el dominio Viejo y el nuevo )
gpfixup /olddns:Cyber.local /newdns:pala.local

# 7. Corregir nombres NetBIOS
gpfixup /oldnb:CYBER /newnb:PALA

# 8. Finalizar proceso
rendom /end
```


## Cambio de Dns
```c
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

#Verificar registros críticos (esto es clave)
nslookup pala.local
nslookup -type=SRV _ldap._tcp.dc._msdcs.pala.local
```



