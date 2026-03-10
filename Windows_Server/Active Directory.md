


# TroubleSh
```sh
✅# **2. Verificar que las ZONAS DNS existen**

Get-Service DNS
Start-Service DNS

Get-DnsServerZone
```

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