```sh
# Ver roles disponibles
Get-WindowsFeature
```

## AD_DS
```sh
# 3. Método por PowerShell (más rápido)
Uninstall-ADDSDomainController

# Si es el único DC (Recomendado)
Uninstall-ADDSDomainController -DemoteOperationMasterRole -ForceRemoval

# Quitar AD DS
Uninstall-WindowsFeature AD-Domain-Services
```

## DHCP
```sh
Uninstall-WindowsFeature DHCP -IncludeManagementTools
```

## DNS_Server
```c
# Ver primero si está instalada:
Get-WindowsFeature DNS

# Eliminar la función:
Uninstall-WindowsFeature -Name DNS -IncludeManagementTools
```

## App Server
```Python
# Ver si está instalado:
Get-WindowsFeature Application-Server

# Eliminar el rol:
Uninstall-WindowsFeature Application-Server

# Si también se quieren quitar las herramientas administrativas:
Uninstall-WindowsFeature Application-Server -IncludeManagementTools
```

## Remote Desktop Services (RDS)
```sh
# Ver si está instalado:
Get-WindowsFeature RDS-RD-Server

# Instalar
Install-WindowsFeature RDS-RD-Server -IncludeManagementTools

# Desinstalar
Uninstall-WindowsFeature RDS-RD-Server
```

## Remote Access
```python
# Ver si está instalado:
Get-WindowsFeature RemoteAccess

# Instalar
Install-WindowsFeature RemoteAccess -IncludeManagementTools

# Desinstalar
Uninstall-WindowsFeature RemoteAccess
```

## Network Access Protection (NAP)
```sh
# Ver si está instalado:
Get-WindowsFeature RemoteAccess

# Instalar
Install-WindowsFeature NPAS -IncludeManagementTools

# Desinstalar
Uninstall-WindowsFeature NPAS
```

##  IIS (Servidor Web)
```sh
# Ver si está instalado:
Get-WindowsFeature Web-Server

# Instalar
Install-WindowsFeature Web-Server -IncludeManagementTools

# Desinstalar
Uninstall-WindowsFeature Web-Server
```



