


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