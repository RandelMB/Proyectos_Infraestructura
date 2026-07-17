# TCPDUMP WATCHGUARD
## CAPTURA BASICA
```bash
-i Optional-10                         # Interfaz específica
-i Optional-11                         # Interfaz específica
-i Optional-12                         # Interfaz específica
-i Optional-13                         # Interfaz específica
-i Optional-14                         # Interfaz específica
-i Optional-3                          # Interfaz específica
-i Optional-4                          # Interfaz específica
-i Optional-5                          # Interfaz específica

-n                                     # Sin resolución DNS
-nn                                    # Sin DNS ni nombres puertos

-v                                     # Verbose básico
-vv                                    # Verbose medio
-vvv                                   # Verbose máximo

-s 0                                   # Captura completa
-c 100                                 # Limitar paquetes

-w captura.pcap                        # Guardar PCAP
-r captura.pcap                        # Leer PCAP

-e                                     # Mostrar cabecera Ethernet

-q                                     # Salida rápida/simple

-x                                     # Payload HEX
-X                                     # HEX + ASCII
-XX                                    # HEX + ASCII + enlace

-tt                                    # Timestamp raw
-ttt                                   # Delta timestamps
-tttt                                  # Fecha completa
```
## HOSTS
```bash
host 192.168.1.10                      # Host origen/destino
src host 192.168.1.10                  # Solo origen
dst host 192.168.1.10                  # Solo destino
host 192.168.1.10 and tcp              # Host + TCP
host 192.168.1.10 and udp              # Host + UDP
host 192.168.1.10 and icmp             # Host + ICMP
host 192.168.1.10 and port 443         # Host + puerto
not host 192.168.1.10                  # Excluir host
```
## REDES

```bash
net 192.168.1.0/24                     # Red completa
src net 10.0.0.0/8                     # Red origen
dst net 172.16.0.0/16                  # Red destino
net 192.168.1.0/24 and tcp             # Red + TCP
net 192.168.1.0/24 and udp             # Red + UDP
net 192.168.1.0/24 and port 80         # Red + puerto
not net 192.168.1.0/24                 # Excluir red
```

## PUERTOS

```bash
port 80                                # Puerto cualquiera
src port 53                            # Puerto origen
dst port 443                           # Puerto destino
portrange 1-1024                       # Rango puertos
tcp port 22                            # TCP SSH
tcp port 80                            # HTTP
tcp port 443                           # HTTPS
udp port 53                            # DNS
udp port 67 or udp port 68             # DHCP
udp port 123                           # NTP
udp port 161                           # SNMP
udp port 162                           # SNMP traps
not port 22                            # Excluir SSH
```

## PROTOCOLOS
```bash
tcp                                    # TCP
udp                                    # UDP
icmp                                   # ICMP
icmp6                                  # ICMPv6
arp                                    # ARP
ip                                     # IPv4
ip6                                    # IPv6
esp                                    # IPSec ESP
gre                                    # GRE
vlan                                   # VLAN
pppoes                                 # PPPoE sesión
```

## VPN
```bash
udp port 500                           # IKE
udp port 4500                          # NAT-T
esp                                    # ESP IPSec
udp port 500 or udp port 4500 or esp  # IPSec completo
gre                                    # GRE VPN
tcp port 1723                          # PPTP
udp port 1701                          # L2TP
udp port 1194                          # OpenVPN
tcp port 443                           # SSL VPN
```

## DNS / DHCP / NTP
```bash
udp port 53                            # DNS UDP
tcp port 53                            # DNS TCP
udp port 67 or udp port 68             # DHCP
udp port 123                           # NTP
```

## HTTP / HTTPS
```bash
tcp port 80                            # HTTP
tcp port 443                           # HTTPS
host 1.1.1.1 and tcp port 443          # HTTPS host específico
-s 0 -nn tcp port 443                  # HTTPS completo
```

## SNMP
```bash
udp port 161                           # SNMP
udp port 162                           # SNMP traps
host 172.25.200.32 and udp port 161    # SNMP host específico
```

## ICMP
```bash
icmp                                   # ICMP
icmp[icmptype] == icmp-echo            # Ping request
icmp[icmptype] == icmp-echoreply       # Ping reply
```

## TCP FLAGS
```bash
tcp[tcpflags] == tcp-syn               # SYN
tcp[tcpflags] == tcp-ack               # ACK
tcp[tcpflags] == tcp-fin               # FIN
tcp[tcpflags] == tcp-rst               # RST
tcp[tcpflags] & tcp-syn != 0           # SYN presente
tcp[tcpflags] & tcp-rst != 0           # RESET presente
```

## VLAN
```bash
vlan                                   # Todo VLAN
vlan 10                                # VLAN 10
vlan and host 10.0.0.1                 # VLAN + host
```

## MAC ADDRESS
```bash
ether host AA:BB:CC:DD:EE:FF           # MAC origen/destino
ether src AA:BB:CC:DD:EE:FF            # MAC origen
ether dst AA:BB:CC:DD:EE:FF            # MAC destino
```

## TAMAÑO PAQUETES

```bash
greater 1000                           # >1000 bytes

less 200                               # <200 bytes

len > 500                              # Longitud >500
```

## OPERADORES LOGICOS

```bash
host 1.1.1.1 and tcp                  # AND

host 1.1.1.1 or host 8.8.8.8          # OR

not tcp                                # NOT

(host 1.1.1.1 and tcp) or icmp         # Agrupación
```

## WATCHGUARD TROUBLESHOOTING

```bash
-i Optional-10 -nn -s 0               # Captura completa

-i Optional-10 -nn host 10.0.0.1      # Host específico

-i Optional-10 -nn tcp port 443       # HTTPS

-i Optional-10 -nn udp port 53        # DNS

-i Optional-10 -nn icmp               # Ping

-i Optional-10 -nn udp port 500 or udp port 4500 or esp   # IPSec

-i Optional-10 -nn not port 22        # Excluir SSH

-i Optional-10 -X host 8.8.8.8        # Payload HEX/ASCII

-i Optional-10 -w soporte.pcap        # Guardar captura
```

## VERIFICACION

```bash
--version                              # Ver versión tcpdump

-D                                     # Interfaces disponibles

-L                                     # Tipos enlace soportados
```