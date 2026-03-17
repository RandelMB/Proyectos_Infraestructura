



# Habilitar RDP

```sh

sudo apt update
sudo apt install -y xrdp

sudo systemctl enable xrdp
sudo systemctl start xrdp

sudo ufw allow 3389
sudo systemctl restart xrdp

```





```sh



## 🔹 Comandos Básicos de Escaneo

# Escaneo profundo de puerto
nmap -sS -sU -p 135 -A -sV 10.0.1.206

# Escaneo TCP SYN (rápido y sigiloso, requiere root)
nmap -sS [IP]
   
# Escaneo TCP Connect (completo, sin root)
nmap -sT [IP]

# Escaneo UDP (lento, para servicios UDP)
nmap -sU [IP] 
 
# Detección de versiones de servicios.
nmap -sV [IP] 

# Detección del sistema operativo.
nmap -O [IP]
   

---

## 🔹 Escaneo de Puertos y Rangos

- `nmap -p 80,443 [IP]`  
  Escanear puertos específicos.  
  🌐 [Extassis Network](https://extassisnetwork.com/tutoriales/como-usar-el-comando-nmap/)

- `nmap -p 1-1000 [IP]`  
  Escanear un rango de puertos.  
  🌐 [RedesZone](https://www.redeszone.net/tutoriales/configuracion-puertos/nmap-escanear-puertos-comandos/)

- `nmap -F [IP]`  
  Escaneo rápido (100 puertos comunes).  
  🌐 [RedesZone](https://www.redeszone.net/tutoriales/configuracion-puertos/nmap-escanear-puertos-comandos/)

- `nmap -p- [IP]`  
  Escanear todos los puertos (1–65535).  
  📺 [YouTube](https://www.youtube.com/watch?v=GDZOsgYiQ44)

- `nmap --top-ports 1000 [IP]`  
  Escanear los 1000 puertos más comunes.  
  🌐 [RedesZone](https://www.redeszone.net/tutoriales/configuracion-puertos/nmap-escanear-puertos-comandos/)

---

## 🔹 Descubrimiento de Hosts

- `nmap -sn [rango IP]`  
  Ping scan (descubre hosts vivos, sin escanear puertos).  
  🌐 [Extassis Network](https://extassisnetwork.com/tutoriales/como-usar-el-comando-nmap/)

- `nmap 192.168.1.0/24`  
  Escanear una subred completa.  
  📺 [YouTube](https://www.youtube.com/watch?v=GDZOsgYiQ44)

- `nmap -iL archivo.txt`  
  Escanear una lista de IPs desde un archivo.  
  📺 [YouTube](https://www.youtube.com/watch?v=GDZOsgYiQ44)

---

## 🔹 Opciones Avanzadas

- `nmap -v [IP]`  
  Modo verboso (más detalles).  
  🌐 [Extassis Network](https://extassisnetwork.com/tutoriales/como-usar-el-comando-nmap/)

- `nmap -A [IP]`  
  Escaneo agresivo (OS, versiones, scripts, traceroute).  
  🌐 [rm-rf](https://rm-rf.es/nmap-linux-uso-ejemplos/)

- `nmap -T4 [IP]`  
  Timing agresivo (escaneo más rápido).  
  📘 [Documentación Oficial](https://nmap.org/man/es/index.html)

- `nmap -oN salida.txt [IP]`  
  Guardar resultados en formato normal.  
  🌐 [Extassis Network](https://extassisnetwork.com/tutoriales/como-usar-el-comando-nmap/)

---

⚠️ Ejecuta Nmap con `sudo` para obtener mayor precisión.  
⚠️ Utiliza siempre Nmap de forma ética y **solo con autorización**.

📘 Documentación oficial:  
https://nmap.org/man/es/index.html
```

