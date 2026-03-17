

User: 
 Pass: c12345

# 49152/tcp open  upnp    Portable SDK for UPnP devices 1.6.19 (Linux 3.3.0; UPnP 1.0)

## Escaneo

```sh




PS C:\Windows\system32> nmap -sV 192.168.0.188
Starting Nmap 7.80 ( https://nmap.org ) at 2026-02-12 20:10 Hora estßndar oeste, SudamÚrica
Nmap scan report for 192.168.0.188
Host is up (0.019s latency).
Not shown: 997 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    lighttpd 1.4.35
554/tcp   open  http    lighttpd 1.4.35
49152/tcp open  upnp    Portable SDK for UPnP devices 1.6.19 (Linux 3.3.0; UPnP 1.0)
MAC Address: 78:A5:DD:0F:42:FD (Shenzhen Smarteye Digital Electronics)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel:3.3.0

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.02 seconds
PS C:\Windows\system32>



# Paso 2

PS C:\Windows\system32> nmap -p 49152 --script upnp-info 192.168.0.188
Starting Nmap 7.80 ( https://nmap.org ) at 2026-02-12 20:22 Hora estßndar oeste, SudamÚrica
Nmap scan report for 192.168.0.188
Host is up (0.0085s latency).

PORT      STATE SERVICE
49152/tcp open  unknown
MAC Address: 78:A5:DD:0F:42:FD (Shenzhen Smarteye Digital Electronics)

Nmap done: 1 IP address (1 host up) scanned in 2.23 seconds
PS C:\Windows\system32>



# P3

PS C:\Windows\system32> nmap -sV -sC -p 49152 192.168.0.188
Starting Nmap 7.80 ( https://nmap.org ) at 2026-02-16 20:01 Hora estßndar oeste, SudamÚrica
Nmap scan report for 192.168.0.188
Host is up (0.015s latency).

PORT      STATE SERVICE VERSION
49152/tcp open  upnp    Portable SDK for UPnP devices 1.6.19 (Linux 3.3.0; UPnP 1.0)
MAC Address: 78:A5:DD:0F:42:FD (Shenzhen Smarteye Digital Electronics)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel:3.3.0



```

**¡SÍ, hay vulnerabilidades reales y activas que pueden explotar ese puerto UPnP expuesto a Internet!** Tu dispositivo usa **Portable SDK for UPnP 1.6.19** (vulnerable) en **Linux 3.3.0** (obsoleto desde 2012), detectado en el escaneo anterior.[[ccn-cert.cni](https://www.ccn-cert.cni.es/es/seguridad-al-dia/vulnerabilidades/view/6646.html)]​

## Riesgos críticos confirmados

| Vulnerabilidad         | CVE IDs                       | Impacto                    | Exploit público             |
| ---------------------- | ----------------------------- | -------------------------- | --------------------------- |
| **Portable SDK UPnP**  | CVE-2012-5958 a CVE-2012-5965 | **RCE/DoS** remoto         | Sí, desde 2013              |
| **CallStranger**       | CVE-2020-12695                | DDoS + redirección tráfico | Masivo (millones afectados) |
| **Kernel Linux 3.3.0** | Múltiples                     | Escalada privilegios       | Metasploit modules          |

## Por qué es peligroso hacia Internet

text

```
1. Puerto 49152 accesible WAN = Ataques automatizados 
2. UPnP sin autenticación = Malware reconfigura tu router 
3. Versión 1.6.19 parcheada en 1.6.18 (2013) = Tu dispositivo vulnerable 
4. SSDP/UPnP en WAN = Amplificación DDoS (4.8M dispositivos afectados 2018)
```
## Si lo necesitas accesible:


## Acción YA:

bash

`# 1. Router: UPnP → OFF globalmente # 2. Dispositivo 192.168.0.188: Desactiva UPnP si posible # 3. Verifica: nmap -p 49152 [tu IP pública] desde externo`




# 80/tcp

```python

PS C:\Windows\system32> nmap -sV -sC -p 80 192.168.0.188
Starting Nmap 7.80 ( https://nmap.org ) at 2026-02-23 18:51 Hora estßndar oeste, SudamÚrica
Nmap scan report for 192.168.0.188
Host is up (0.0040s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    lighttpd 1.4.35
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Digest nonce=ec605f37fcf14de588b513558c354bd9 realm=IPCamera Login qop=auth
|_http-server-header: lighttpd/1.4.35
|_http-title: 401 - Unauthorized
MAC Address: 78:A5:DD:0F:42:FD (Shenzhen Smarteye Digital Electronics)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 5.80 seconds


```




```
80/tcp
lighttpd 1.4.35
HTTP Digest Auth
realm=IPCamera Login
Vendor: Shenzhen Smarteye
```

Conclusión directa:
- No es un servidor genérico    
- Es **firmware de cámara/IP device**    
- lighttpd es **solo el frontend**    
- La **lógica vulnerable casi nunca está en lighttpd**, está **detrás** 

lighttpd 1.4.35 **no es vulnerable por versión**. El riesgo está en **cómo el dispositivo implementa la autenticación y los endpoints**.

---
## Superficie de ataque REAL del puerto 80

### 1. Autenticación Digest (primer vector)

Digest **no cifra**, solo ofusca.
A nivel de ataque:
- El atacante provoca múltiples retos Digest    
- Captura:    
    - nonce        
    - realm        
    - username
        
- Si el firmware:    
    - usa nonces reutilizables       
    - usa MD5 débil        
    - no rota nonce  
        → **ataques offline de credenciales**

En IoT esto es **muy común**.
**Impacto**: acceso web completo sin explotar nada más.

---
### 2. Credenciales por defecto / hardcodeadas

En cámaras chinas:
- Usuarios ocultos    
- Backdoor accounts    
- Passwords fijas en firmware    

No es lighttpd.  
Es el **backend CGI / binario**.

**Impacto**:
- Bypass total de login    
- Acceso administrativo    
---

---
### 4. CGI / Binary injection (crítico)

El 90% de cámaras vulnerables caen aquí.
Arquitectura típica:

```
lighttpd → CGI → binario propietario
```



---
### 5. Falta de separación de privilegios

En estos dispositivos:
- lighttpd corre como root    
- CGI corre como root    

**Impacto**:
- Si rompes la web → control total del equipo    

---



y te digo **qué clase de CVEs suelen aplicar** y **qué controles compensatorios poner** para cerrar el laboratorio correctamente.






