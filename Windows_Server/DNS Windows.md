
# ABRIR HOSTS CON EDITORES

```powershell
notepad C:\Windows\System32\drivers\etc\hosts                         # abrir con Bloc de notas
notepad.exe C:\Windows\System32\drivers\etc\hosts                     # variante explícita

code C:\Windows\System32\drivers\etc\hosts                            # abrir con VSCode
code --reuse-window C:\Windows\System32\drivers\etc\hosts             # reutilizar ventana VSCode

powershell -Command "notepad $env:windir\System32\drivers\etc\hosts"  # desde PowerShell
start notepad C:\Windows\System32\drivers\etc\hosts                   # abrir usando start

Start-Process notepad "C:\Windows\System32\drivers\etc\hosts"         # abrir proceso
Start-Process code "C:\Windows\System32\drivers\etc\hosts"            # abrir en VSCode

ise C:\Windows\System32\drivers\etc\hosts                             # PowerShell ISE
```

# AGREGAR ENTRADAS MANUALMENTE
```txt
127.0.0.1 midominio.local
127.0.0.1 app.local api.local
192.168.1.10 nas.local
10.0.0.5 proxmox.local
172.16.0.2 docker-registry.local
```

# AGREGAR ENTRADAS DESDE POWERSHELL
```powershell
Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "`n127.0.0.1 midominio.local"     # agregar línea simple

echo "`n127.0.0.1 midominio.local" >> C:\Windows\System32\drivers\etc\hosts                         # append usando echo

"`n127.0.0.1 midominio.local" | Out-File -Append C:\Windows\System32\drivers\etc\hosts             # append usando Out-File

Set-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value (Get-Content C:\Windows\System32\drivers\etc\hosts)   # reescribir archivo

$hostEntry = "`n127.0.0.1 midominio.local"
Add-Content -Path "$env:windir\System32\drivers\etc\hosts" -Value $hostEntry                        # usando variable

[System.IO.File]::AppendAllText("$env:windir\System32\drivers\etc\hosts","`n127.0.0.1 midominio.local")   # usando .NET
```

# ELIMINAR ENTRADAS

```powershell
(Get-Content C:\Windows\System32\drivers\etc\hosts) | Where-Object {$_ -notmatch "midominio.local"} | Set-Content C:\Windows\System32\drivers\etc\hosts   # eliminar entrada

(gc C:\Windows\System32\drivers\etc\hosts) -replace "127.0.0.1 midominio.local","" | sc C:\Windows\System32\drivers\etc\hosts   # replace vacío
```

# RESPALDAR Y RESTAURAR

```powershell
Copy-Item C:\Windows\System32\drivers\etc\hosts C:\Windows\System32\drivers\etc\hosts.bak   # backup

Copy-Item C:\Windows\System32\drivers\etc\hosts.bak C:\Windows\System32\drivers\etc\hosts -Force   # restaurar

type C:\Windows\System32\drivers\etc\hosts > hosts-backup.txt   # exportar backup texto
```

# VERIFICAR HOSTS

```powershell
Get-Content C:\Windows\System32\drivers\etc\hosts                    # mostrar contenido
cat C:\Windows\System32\drivers\etc\hosts                            # alias cat
gc C:\Windows\System32\drivers\etc\hosts                             # alias gc

Select-String "midominio.local" C:\Windows\System32\drivers\etc\hosts   # buscar dominio

Test-Path C:\Windows\System32\drivers\etc\hosts                      # verificar existencia
```

# PROBAR RESOLUCION DNS

```powershell
ping midominio.local                    # probar resolución
nslookup midominio.local                # consultar DNS
Resolve-DnsName midominio.local         # resolver dominio
tracert midominio.local                 # trazar ruta
```

# LIMPIAR CACHE DNS

```powershell
ipconfig /flushdns                      # limpiar caché DNS
Clear-DnsClientCache                    # PowerShell moderno

ipconfig /displaydns                    # mostrar caché DNS
Get-DnsClientCache                      # listar caché DNS
```

# ABRIR POWERSHELL COMO ADMIN

```powershell
Start-Process powershell -Verb runAs    # elevar PowerShell
Start-Process pwsh -Verb runAs          # PowerShell 7 administrador
```

# PERMISOS Y DIAGNOSTICO

```powershell
whoami /groups                          # verificar privilegios
net session                             # comprobar admin
icacls C:\Windows\System32\drivers\etc\hosts   # permisos archivo

attrib C:\Windows\System32\drivers\etc\hosts   # atributos archivo
attrib -r C:\Windows\System32\drivers\etc\hosts   # quitar solo lectura
```

# RESETEAR HOSTS DEFAULT

```powershell
@"
# Copyright (c) Microsoft Corp.
127.0.0.1 localhost
::1 localhost
"@ | Set-Content C:\Windows\System32\drivers\etc\hosts   # restaurar default
```