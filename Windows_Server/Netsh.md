
# FIREWALL – netsh advfirewall

### Activar o desactivar firewall

```
netsh advfirewall set allprofiles state on
netsh advfirewall set allprofiles state off
netsh advfirewall set domainprofile state on
netsh advfirewall set privateprofile state on
netsh advfirewall set publicprofile state on
netsh advfirewall set domainprofile state off
netsh advfirewall set privateprofile state off
netsh advfirewall set publicprofile state off
```

### Ver estado del firewall
```
netsh advfirewall show allprofiles
netsh advfirewall show domainprofile
netsh advfirewall show privateprofile
netsh advfirewall show publicprofile
```

### Restaurar configuración
```
netsh advfirewall reset
```

---

### Crear reglas

```
netsh advfirewall firewall add rule name="Regla1" dir=in action=allow protocol=TCP localport=80
netsh advfirewall firewall add rule name="Regla2" dir=in action=block protocol=TCP localport=445
netsh advfirewall firewall add rule name="Regla3" dir=out action=allow protocol=UDP localport=53
netsh advfirewall firewall add rule name="Regla4" dir=in action=allow program="C:\programa.exe"
```

Variantes:

```
dir=in
dir=out
action=allow
action=block
protocol=TCP
protocol=UDP
protocol=ICMPv4
protocol=ICMPv6
profile=domain
profile=private
profile=public
enable=yes
enable=no
```

Ejemplos completos

```
netsh advfirewall firewall add rule name="HTTP" dir=in action=allow protocol=TCP localport=80 profile=any
netsh advfirewall firewall add rule name="RDP" dir=in action=allow protocol=TCP localport=3389
netsh advfirewall firewall add rule name="DNS" dir=out action=allow protocol=UDP remoteport=53
```

---

### Mostrar reglas

```
netsh advfirewall firewall show rule name=all
netsh advfirewall firewall show rule name="HTTP"
netsh advfirewall firewall show rule name=all dir=in
netsh advfirewall firewall show rule name=all dir=out
```

---

### Modificar reglas

```
netsh advfirewall firewall set rule name="HTTP" new enable=yes
netsh advfirewall firewall set rule name="HTTP" new enable=no
netsh advfirewall firewall set rule name="HTTP" new action=block
netsh advfirewall firewall set rule name="HTTP" new action=allow
```

---

### Eliminar reglas

```
netsh advfirewall firewall delete rule name="HTTP"
netsh advfirewall firewall delete rule name=all protocol=TCP localport=80
```

---

### Exportar e importar firewall

```
netsh advfirewall export C:\firewall.wfw
netsh advfirewall import C:\firewall.wfw
```

---

### Logging

```
netsh advfirewall set currentprofile logging filename C:\firewall.log
netsh advfirewall set currentprofile logging maxfilesize 4096
netsh advfirewall set currentprofile logging droppedconnections enable
netsh advfirewall set currentprofile logging allowedconnections enable
```

---

# WLAN – netsh wlan

### Mostrar información wifi

```
netsh wlan show interfaces
netsh wlan show drivers
netsh wlan show networks
netsh wlan show networks mode=bssid
netsh wlan show profiles
```

---

### Ver perfiles wifi

```
netsh wlan show profile
netsh wlan show profile name="WIFI"
netsh wlan show profile name="WIFI" key=clear
```

---

### Conectarse o desconectarse

```
netsh wlan connect name="WIFI"
netsh wlan disconnect
```

Variantes:

```
netsh wlan connect ssid="WIFI" name="WIFI"
netsh wlan connect name="WIFI" interface="Wi-Fi"
```

---

### Borrar perfiles wifi

```
netsh wlan delete profile name="WIFI"
netsh wlan delete profile name=*
```

---

### Exportar perfiles

```
netsh wlan export profile key=clear folder=C:\wifi
netsh wlan export profile name="WIFI" folder=C:\wifi
```

---

### Importar perfil wifi

```
netsh wlan add profile filename="wifi.xml"
```

---

### Escaneo de redes

```
netsh wlan show networks
netsh wlan show networks mode=bssid
```

---

### Configuración interfaz

```
netsh wlan set autoconfig enabled=yes interface="Wi-Fi"
netsh wlan set autoconfig enabled=no interface="Wi-Fi"
```

---

### Hosted Network (Hotspot)

Permitir hotspot

```
netsh wlan set hostednetwork mode=allow ssid=MiRed key=12345678
```

Iniciar hotspot

```
netsh wlan start hostednetwork
```

Detener hotspot

```
netsh wlan stop hostednetwork
```

Ver estado

```
netsh wlan show hostednetwork
```

Deshabilitar hotspot

```
netsh wlan set hostednetwork mode=disallow
```

---

### Filtrado de redes wifi

Permitir solo ciertas redes

```
netsh wlan add filter permission=allow ssid="MiRed" networktype=infrastructure
```

Bloquear redes

```
netsh wlan add filter permission=block ssid="RedMala" networktype=infrastructure
```

Ver filtros

```
netsh wlan show filters
```

Eliminar filtro

```
netsh wlan delete filter permission=block ssid="RedMala"
```

---

### Configuración avanzada

```
netsh wlan set profileparameter name="WIFI" connectionmode=auto
netsh wlan set profileparameter name="WIFI" connectionmode=manual
```

```sh
# Ver configuración actual

ipconfig

Ver más detalles:

ipconfig /all

---

# Cambiar IP usando CMD (netsh)

Primero ver el nombre de la interfaz:

netsh interface ipv4 show interfaces

Ejemplo si la interfaz se llama **Ethernet**.

---

## Asignar IP estática

netsh interface ipv4 set address name="Ethernet" static 192.168.1.50 255.255.255.0 192.168.1.1

Formato:

netsh interface ipv4 set address name="INTERFAZ" static IP MASCARA GATEWAY

---

## Cambiar DNS

netsh interface ipv4 set dns name="Ethernet" static 8.8.8.8

DNS secundario:

netsh interface ipv4 add dns name="Ethernet" 8.8.4.4 index=2

---

# Volver a DHCP (automático)

IP automática:

netsh interface ipv4 set address name="Ethernet" source=dhcp

DNS automático:

netsh interface ipv4 set dns name="Ethernet" source=dhcp

---

# Renovar dirección DHCP

ipconfig /release  
ipconfig /renew

---

# Limpiar cache DNS

ipconfig /flushdns

---
```