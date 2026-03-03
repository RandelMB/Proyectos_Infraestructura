Si ya tienes el dominio funcionando en **Windows Server**, hazlo por PowerShell así, directo.

---

# 1️⃣ Crear la zona inversa

Primero identifica tu red.  
Ejemplo: 192.168.10.0/24

En PowerShell (como administrador):

```powershell
Add-DnsServerPrimaryZone -NetworkID "192.168.10.0/24" -ReplicationScope "Domain"
```

Eso crea la zona inversa 10.168.192.in-addr.arpa.

---

# 2️⃣ Crear 5 registros A

Ejemplo dominio: midominio.local  
IP del DNS: 192.168.10.1

```powershell
Add-DnsServerResourceRecordA -Name "pc1" -ZoneName "midominio.local" -IPv4Address "192.168.10.10"
Add-DnsServerResourceRecordA -Name "pc2" -ZoneName "midominio.local" -IPv4Address "192.168.10.11"
Add-DnsServerResourceRecordA -Name "pc3" -ZoneName "midominio.local" -IPv4Address "192.168.10.12"
Add-DnsServerResourceRecordA -Name "pc4" -ZoneName "midominio.local" -IPv4Address "192.168.10.13"
Add-DnsServerResourceRecordA -Name "pc5" -ZoneName "midominio.local" -IPv4Address "192.168.10.14"
```

Eso crea:
- pc1.midominio.local    
- pc2.midominio.local    
- etc.
    

---

# 3️⃣ Crear 2 Alias (CNAME)

Alias apuntando a hosts ya creados:

```powershell
Add-DnsServerResourceRecordCName -Name "servidorweb" -ZoneName "midominio.local" -HostNameAlias "pc1.midominio.local"
Add-DnsServerResourceRecordCName -Name "archivos" -ZoneName "midominio.local" -HostNameAlias "pc2.midominio.local"
```

Ahora:

- servidorweb.midominio.local → apunta a pc1    
- archivos.midominio.local → apunta a pc2    

---

# 4️⃣ Crear los PTR en la zona inversa

Para cada IP:

```powershell
Add-DnsServerResourceRecordPtr -Name "10" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc1.midominio.local"
Add-DnsServerResourceRecordPtr -Name "11" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc2.midominio.local"
Add-DnsServerResourceRecordPtr -Name "12" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc3.midominio.local"
Add-DnsServerResourceRecordPtr -Name "13" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc4.midominio.local"
Add-DnsServerResourceRecordPtr -Name "14" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc5.midominio.local"
```

Esto hace que si consultas la IP, te devuelva el nombre.

---

# 5️⃣ ¿Qué es IPConfig?

Es un comando de red de Windows que muestra y controla la configuración IP.

Comandos principales:

```
ipconfig
```

Muestra IP, máscara, gateway.

```
ipconfig /all
```

Muestra todo (DNS, DHCP, MAC).

```
ipconfig /flushdns
```

Borra la caché DNS del equipo.

```
ipconfig /registerdns
```

Fuerza al equipo a registrar su nombre en el DNS.

```
ipconfig /release
```

Libera la IP DHCP.

```
ipconfig /renew
```

Pide nueva IP al DHCP.

---

# 6️⃣ ¿Qué es Nslookup?

Es una herramienta para consultar servidores DNS.

Ejemplos:

```
nslookup pc1.midominio.local
```

Te dice qué IP tiene.

```
nslookup 192.168.10.10
```

Te dice el nombre si existe PTR.

También puedes consultar DNS externos:

```
nslookup google.com 8.8.8.8
```

Sirve para:

- Verificar si un registro existe
    
- Comprobar si la zona inversa funciona
    
- Diagnosticar problemas DNS
    

---

# Resumen

Lo que te piden es:

- Zona directa → nombre a IP (A y CNAME)
    
- Zona inversa → IP a nombre (PTR)
    
- Entender comandos básicos de diagnóstico DNS en Windows
    

Si quieres, te lo estructuro como informe listo para entregar.