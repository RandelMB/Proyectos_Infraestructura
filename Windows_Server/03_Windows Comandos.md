

# Activación de Windows

https://massgrave.dev/



# impresoras
```sh

# Listado de print
wmic printer list brief
printui.exe /s /t2

# eliminar Impresoras
printui.exe /dl /n "NombreImpresora"


```




# Redes
```sh
# ---Validar puertos abiertos
curl portquiz.net:22

curl -4 ifconfig.me

----------------------------


netstat -abno
arp -a
route print

------------



## vaciar la caché DNS

nslookup youtube.com
nslookup youtube.com 1.1.1.1

while ($true) { nslookup youtube.com 1.1.1.1; Start-Sleep -Seconds 2 }
while ($true) { nslookup youtube.com 10.0.1.10; Start-Sleep -Seconds 2 }

ipconfig /flushdns



-------------------------------

dism /Online /Enable-Feature /FeatureName:OpenSSH-Server
net start sshd

dism /Online /Enable-Feature /FeatureName:TelnetClient
net start sshd

-------------------------

net share  -  para ver carpetas compartidas
net share carpeta_compart      -      ver detalles de carpeta 
net share carpeta_compart=ruta_completa [opciones]
net share carpeta_compart/delete



netsh interface show interface
netsh interface ip set address "NombreDeTuInterfaz" static 172.16.0.X 255.255.255.0 172.16.0.6
netsh interface ip set dns "NombreDeTuInterfaz" static 172.16.0.1
netsh interface ip add dns "NombreDeTuInterfaz" 8.8.8.8 index=2

-------------------------------

## Ruta de Dns


- En Linux/macOS: `/etc/hosts`
    
- En Windows: 
  C:\Windows\System32\drivers\etc\hosts


```



## Comando general para grabar pantalla en Windows con ffmpeg

```

ffmpeg -f gdigrab -framerate 30 -video_size 1920x1080 -i desktop -c:v libx264 -preset ultrafast -crf 18 -pix_fmt yuv420p "E:\DESCARGAS\VIDEO\TUTORIALES\grabacion_1080p3.mp4"


```


## DiskPart Modo lectura o escritura


```

select disk 1

attributes disk set readonly

attributes disk clear readonly

```




## Generar la clave SSH desde Window

```python

# --GENERADOR DE LLAVES (SELECCIONA 1)-- #
ssh-keygen -t rsa -b 4096 -C "ejemp@ejemplo.com"
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/id_ecdsa_521
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519


# --
type $env:USERPROFILE\.ssh\id_rsa.pub


# -- EN UBUNTU --#

chmod 700 ~/.ssh/authorized_keys

nano ~/.ssh/authorized_keys
sudo systemctl restart ssh


```


| Formato           | Extensión típica                                             | Usado por / compatibilidad            | Comentario                                                                        |
| ----------------- | ------------------------------------------------------------ | ------------------------------------- | --------------------------------------------------------------------------------- |
| **PPK**           | `.ppk`                                                       | PuTTY, WinSCP                         | Formato propietario de PuTTY; no se puede usar directamente en OpenSSH.           |
| **PEM**           | `.pem`                                                       | OpenSSH, OpenSSL, servidores web      | Base64 con encabezados `-----BEGIN PRIVATE KEY-----`; muy usado en TLS/SSL y SSH. |
| **OpenSSH**       | `id_rsa`, `id_ed25519` (sin extensión o `.pub` para pública) | OpenSSH, Linux/macOS, Windows OpenSSH | Formato nativo de OpenSSH. Claves privadas y públicas separadas.                  |
| **PKCS#8**        | `.key`, `.pem`                                               | OpenSSL, Java, algunos servicios      | Puede contener claves privadas en estándar interoperable; admite cifrado.         |
| **PKCS#12 / PFX** | `.p12`, `.pfx`                                               | Windows, OpenSSL                      | Contiene certificado + clave privada + cadena completa, usado para TLS/SSL.       |
| **CER / CRT**     | `.cer`, `.crt`                                               | Windows, Linux                        | Certificados públicos; no contienen la clave privada.                             |
| **DER**           | `.der`                                                       | Java, OpenSSL                         | Formato binario de certificado; alternativa a PEM.                                |



## Ejecutar Comands

```python
gpedit.msc #– Editor de políticas de grupo local
eventvwr.msc #– Visor de eventos

control firewall.cpl
sysdm.cpl #– Propiedades del sistema
ncpa.cpl #– Conexiones de red
netplwiz #– Cuentas de usuario
appwiz.cpl #– Programas y características (desinstalar programas)
mmsys.cpl #- Audio
powercfg.cpl #- energia

regedit #– Editor del registro
services.msc #– Servicio
msconfig #– Configuración del sistema
msinfo32 #– Información del sistema
resmon #– Monitor de recursos
powercfg.cpl #– Opciones de energía
main.cpl #– Configuración del ratón
chkdsk #– Comprobar disco
```

## Power /(No Comprobado)

```
powercfg -change -standby-timeout-ac 0

powercfg /hibernate off
```

## ShutDown

```python
#Apagar en 10 minutos (600 segundos):
shutdown /s /t 600

#Apagar en 20 minutos (1200 segundos):
shutdown /s /t 1200

#Apagar en 30 minutos (1800 segundos):
shutdown /s /t 1800

#Apagar en 40 minutos (2400 segundos):
shutdown /s /t 2400

#Apagar en 50 minutos (3000 segundos):
shutdown /s /t 3000

#Apagar en 1 hora (3600 segundos):
shutdown /s /t 3600

#Apagar en 1 hora y 10 minutos (4200 segundos):
shutdown /s /t 4200

#Apagar en 1 hora y 20 minutos (4800 segundos):
shutdown /s /t 4800

#Apagar en 1 hora y 30 minutos (5400 segundos):
shutdown /s /t 5400

#Apagar en 1 hora y 40 minutos (6000 segundos):
shutdown /s /t 6000

#Apagar en 1 hora y 50 minutos (6600 segundos):
shutdown /s /t 6600

#Apagar en 2 horas (7200 segundos):
shutdown /s /t 7200

#Apagar en 2 horas y 10 minutos (7800 segundos):
shutdown /s /t 7800

#Apagar en 2 horas y 20 minutos (8400 segundos):
shutdown /s /t 8400

#Apagar en 2 horas y 30 minutos (9000 segundos):
shutdown /s /t 9000

#Apagar en 2 horas y 40 minutos (9600 segundos):
shutdown /s /t 9600

#Apagar en 2 horas y 50 minutos (10200 segundos):
shutdown /s /t 10200

#Apagar en 3 horas (10800 segundos):
shutdown /s /t 10800
```
# Usuarios y Grupos Locales
```sh
query user     #-  Ver ususarios
net users      #-  Ver ususarios
Get-LocalUser  #-  Ver ususarios

# -Para ver nombre de pc y dominio
net config workstation

-------------------------------------------------------------------------

# Usuarios (net user)

net user → Lista todas las cuentas  
net user <usuario> → Muestra detalles  
net user <usuario> <contraseña> → Cambia contraseña  
net user /? → Ayuda

net user <usuario> <contraseña> /add → Crear usuario  
net user <usuario> /delete → Eliminar usuario  
net user <usuario> /active:yes|no → Activar o desactivar

# Opciones:  
/domain → Aplicar en dominio  
/times:all → Limitar horarios, Acceso 24/7 (o `/times:horario` para limitar)
/expires → Fecha de expiración  
/passwordreq:yes|no → Obliga tener contraseña
/passwordchg:yes - Permite cambiar contraseña propia.
/expires:never - sin fecha de caducidad

# Grupos (net localgroup)

net localgroup → Listar grupos  
net localgroup <grupo> → Ver miembros  
net localgroup /? → Ayuda

net localgroup <grupo> /add → Crear grupo  
net localgroup <grupo> /delete → Eliminar grupo  
net localgroup <grupo> /comment:"texto" → Agregar descripción

net localgroup <grupo> <usuario> /add → Agregar usuario  
net localgroup <grupo> <usuario> /delete → Quitar usuario

# Ver grupos y membresías

net user <usuario> → Ver grupos del usuario  
net localgroup <grupo> → Ver miembros del grupo  
net localgroup → Ver todos los grupos


# Cuentas

net accounts /maxpwage:30 - tiempo que puede estar sin logearse
net accounts /minpwlen:10 - Longitud minisma para password

```


# Domnio

```python
# Renombrar Equipos 
Rename-Computer -NewName "Tecnologia"
Rename-Computer -NewName <nuevo_nombre> -Restart

# Agregar A WorkGroup
Add-Computer -WorkgroupName <nombre_del_Workgroup>
Add-Computer -WorkgroupName W -Restart

# Agregar al Dominio
Add-Computer -DomainName <nombre_del_dominio>
Add-Computer -DomainName arsfuturo.com.do -Restart
Add-Computer -DomainName cybersupports.net -Restart 
-----------------------------------------------------------------------

# Crear Usuario Local
net user Camila /add

New-LocalUser -Name mercadeo -Password ([[Investigacion/Server/ConvertTo-SecureString.md|ConvertTo-SecureString]] "odontodom" -AsPlainText -Force)

# -eliminar usuario

Remove-LocalUser -Name "Estudiante"
---------------------------------------------------
- cambiar usuario a administrador

net localgroup Administrators odont /add

Add-LocalGroupMember -Group "Administrators" -Member "mercadeo"

---------------------------------------------------

en caso de cerrar el explorador de archivos
start explorer.exe

------------------------------------------------------

```

# Window Update
```sh
###### Para obtener logs de windows Updat
Get-WindowsUpdateLog
##### 1. Detener servicios relacionados
Stop-Service wuauserv -Force
Stop-Service bits -Force
Stop-Service cryptsvc -Force
Stop-Service msiserver -Force

##### 2. Borrar las carpetas de caché
Remove-Item -Path "C:\Windows\SoftwareDistribution\DataStore\*" -Recurse -Force
Remove-Item -Path "C:\Windows\SoftwareDistribution\Download\*" -Recurse -Force
Remove-Item -Path "C:\Windows\System32\catroot2\*" -Recurse -Force

###### 3. Iniciar servicios nuevamente
Start-Service wuauserv
Start-Service bits
Start-Service cryptsvc
Start-Service msiserver

###### 4. Forzar búsqueda de actualizaciones
wuauclt.exe /detectnow
wuauclt.exe /updatenow

www.update.microsoft.com
download.windowsupdate.com
*.delivery.mp.microsoft.com

```
--------


## Asistente de Winodws 11 UPDATE

https://go.microsoft.com/fwlink/?linkid=2171764



---------


# Script Startup /(No Probado)
 ```r
 # Definir la acción
$Action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument '-ExecutionPolicy Bypass -File "C:\Scripts\audio.ps1"'

 # Definir el disparador (cuando inicia sesión el usuario)
$Trigger = New-ScheduledTaskTrigger -AtLogOn
 
 # Registrar la tarea
Register-ScheduledTask -Action $Action -Trigger $Trigger -TaskName "ForzarAudioPredeterminado" -Description "Establece siempre los altavoces como salida predeterminada"
 ```

# Acceso Directo para navegador en especifico

```sh
%windir%\explorer.exe microsoft-edge:http://Dominion.web/talcosa/

### 1️⃣ Google Chrome
"C:\Program Files\Google\Chrome\Application\chrome.exe" "http://Dominion.web/talcosa/"

### 2️⃣ Mozilla Firefox
"C:\Program Files\Mozilla Firefox\firefox.exe" "http://Dominion.web/talcosa/"

### 3️⃣ Opera
"C:\Program Files\Opera\launcher.exe" "http://Dominion.web/talcosa/"

### 4️⃣ Brave
"C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe" "http://Dominion.web/talcosa/"
```

# Relación de confianza 

```r
Test-ComputerSecureChannel

Test-ComputerSecureChannel -Repair -Credential dfuturo\  

Reset-ComputerMachinePassword -Credential dfuturo\

# Sincronizar hora (No me funciona) 
net stop w32time
net start w32time
w32tm /resync /force

```

# Generar Certificado Windows
```python
# Descargar y entrar al directorio
cd "E:\PROGRAMAS\WIN-ACME\"
.\wacs.exe --target manual --host cybersupports.net,webmin.cybersupports.net --validation http-01 --validationmode selfhosting --store pemfiles --installation none --accepttos

```

Explicación:

- `--target manual` → introduces dominios manualmente.
- `--host` → dominios separados por coma.
- `--validation selfhttp` → usa un pequeño servidor HTTP temporal en Windows para el desafío HTTP-01 (puerto 80 debe estar libre).
- `--store pemfiles` → genera archivos PEM (privkey.pem, cert.pem, fullchain.pem) directamente, listo para usar en Webmin o Linux.
- `--install none` → no instala en IIS ni Windows; solo genera los archivos.
- `--accepttos` → acepta los TOS de Let’s Encrypt.

**Notas importantes:**

- El puerto 80 debe estar libre durante el desafío.
- El certificado generado se renueva con tarea programada si quieres automatizar.
- Los archivos `.pem` resultantes se pueden copiar a Linux/Webmin tal cual.


# LLDP En Windows


```python

# - INSTALAR -
Install-Module -Name PSDiscoveryProtocol
# o
Install-PSResource -Name PSDiscoveryProtocol

# -PROBAR FUNCIONAMIENTO-#
Invoke-DiscoveryProtocolCapture -Type LLDP

# -EJECUCION- #
$Packet = Invoke-DiscoveryProtocolCapture -Type LLDP -Duration 60 -Force

Get-DiscoveryProtocolData -Packet $Packet



# CDP

[WinCDP-win32-2.0.0.zip - Google Drive](https://drive.google.com/file/d/17zrvlmHytYG4iwXjk5cxCFROg9TYLljg/view)

# LLDP LinkSkippyl
https://github.com/andkrau/LinkSkippy

```

# Deteccion de Server DHCP

```
Install-WindowsFeature DHCP -IncludeManagementTools


```