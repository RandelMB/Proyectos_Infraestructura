
# TroubleShoting

```r
New-ADUser -Name "Juan Perez" -GivenName "Juan" -Surname "Perez" -SamAccountName "jperez" -UserPrincipalName "jperez@dominio.local" -Path "CN=Users,DC=dominio,DC=local" -AccountPassword (ConvertTo-SecureString "Password123*" -AsPlainText -Force) -Enabled $true
```

```powershell
Import-Module ActiveDirectory

$dominio = "empresa.local"
$ou = "CN=Users,DC=empresa,DC=local"
$password = ConvertTo-SecureString "Password123*" -AsPlainText -Force

for ($i=1; $i -le 10; $i++) {

    $usuario = "user$i"
    $nombre = "User $i"
    $upn = "$usuario@$dominio"

    New-ADUser `
        -Name $nombre `
        -GivenName "User" `
        -Surname "$i" `
        -SamAccountName $usuario `
        -UserPrincipalName $upn `
        -Path $ou `
        -AccountPassword $password `
        -Enabled $true
}
```

# Copiar permisos de usuarios
```c
$UserC = "eddy.castillo"
$UserP = "rafael.agramonte"

Get-ADUser -Identity $UserC -Properties memberof | 
    Select-Object memberof -ExpandProperty memberof | 
    Add-ADGroupMember -Members $UserP
```


```sh
# Crear las OU
New-ADOrganizationalUnit -Name "Pala Pizza" -Path "DC=pala,DC=local"

# Crear las OU detro de una OU 
New-ADOrganizationalUnit -Name "Tecnologia" -Path "OU=Pala Pizza,DC=pala,DC=local"  

# 3. Crear usuarios (OU Tecnología)
New-ADUser -Name "Juan Perez" -SamAccountName jperez -AccountPassword (ConvertTo-SecureString "Admin123*" -AsPlainText -Force) -Enabled $true -Path "OU=Tecnologia,OU=Pala Pizza,DC=pala,DC=local"

# 3. Crear 5 grupos de seguridad
New-ADGroup -Name "Seguridad_Tecnologia" -GroupScope Global -GroupCategory Security -Path "OU=Pala Pizza,DC=pala,DC=local"

# 4. Crear 5 grupos de distribución
New-ADGroup -Name "Distribucion_GestionHumana" -GroupScope Global -GroupCategory Distribution -Path "OU=Pala Pizza,DC=pala,DC=local"
```