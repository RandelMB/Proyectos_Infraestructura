# Deteccion de Server DHCP (nmap)
```sh
nmap --script broadcast-dhcp-discover             # Script de nmap
bootp                                             # Wireshark
```
# DETECTAR SERVIDORES DHCP (WINDOWS)
```bash
ipconfig /all                                  # ver servidor DHCP asignado
ipconfig /renew                                # forzar solicitud DHCP

netsh dhcp show server                         # listar servidores DHCP (dominio)

powershell -command "Get-DhcpServerInDC"       # detectar DHCP en AD

arp -a                                         # ver gateway/DHCP en cache

nmap --script broadcast-dhcp-discover -e Ethernet   # detectar DHCP en red (requiere nmap)
```
# DETECTAR SERVIDORES DHCP (LINUX)
```bash
ip a                                           # ver interfaz activa
ip route                                       # ver gateway

sudo dhclient -v                               # solicitar DHCP y ver servidor

sudo nmap --script broadcast-dhcp-discover     # escanear servidores DHCP

sudo tcpdump -i eth0 port 67 or port 68 -n     # capturar tráfico DHCP

sudo nmap -sU -p 67 --script=dhcp-discover     # detectar DHCP vía UDP
```
# HERRAMIENTAS ESPECIFICAS
```bash
sudo apt install -y nmap tcpdump dhcping       # instalar herramientas

sudo dhcping -c 192.168.1.100 -s 192.168.1.1   # probar servidor DHCP específico

sudo dhcpdump -i eth0                          # analizar respuestas DHCP
```
# VERIFICACION
```bash
ipconfig /all | findstr /i dhcp                # validar DHCP en Windows
grep -i dhcp /var/lib/dhcp/dhclient.leases    # ver servidor DHCP en Linux

sudo tcpdump -nn -i eth0 port 67               # confirmar respuestas DHCP
```