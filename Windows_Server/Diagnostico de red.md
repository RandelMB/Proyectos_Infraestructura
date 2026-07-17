# PING DESDE INTERFAZ ESPECIFICA WINDOWS
```bash
ipconfig                     # listar interfaces y direcciones IP disponibles
ping -S 192.168.1.10 8.8.8.8 # usar IP origen especifica (interface deseada)
ping -S 10.0.0.5 google.com  # alternativa usando dominio
```
# VERIFICACION
```bash
arp -a                       # verificar interfaz usada por la IP origen
tracert -d 8.8.8.8           # confirmar ruta tomada
netstat -rn                  # validar tabla de rutas
```