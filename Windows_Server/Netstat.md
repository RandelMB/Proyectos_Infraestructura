
# NETSTAT (USO OPTIMIZADO

Explicación mínima: conjunto limpio y útil para diagnóstico real (sin redundancia).

```bash
# ----------VISUALIZACION BASICA------------

netstat -an                          # todas las conexiones en formato numerico
netstat -a                           # conexiones + puertos en escucha
netstat -n                           # sin resolucion DNS


# ----------PROCESOS Y PUERTOS------------

netstat -ano                         # conexiones + PID
netstat -abno                        # conexiones + PID + ejecutable (admin)


# ----------FILTROS CLAVE------------

netstat -ano | findstr :80           # filtrar puerto 80
netstat -ano | findstr :443          # filtrar puerto 443
netstat -ano | findstr LISTENING     # puertos en escucha
netstat -ano | findstr ESTABLISHED   # conexiones activas


# ----------MONITOREO EN TIEMPO REAL------------

netstat -ano 2                       # refresco cada 2 segundos
netstat -ano 5 | findstr :443        # monitoreo puerto 443


# ----------ESTADISTICAS------------

netstat -e                           # trafico interfaz (bytes)
netstat -s                           # estadisticas por protocolo
netstat -sp tcp                      # estadisticas TCP


# ----------ENRUTAMIENTO------------
netstat -r                           # tabla de rutas
route print                          # equivalente


# ----------DIAGNOSTICO AVANZADO------------

netstat -q                           # conexiones TCP extendidas
netstat -y                           # plantilla TCP
netstat -f -o                        # FQDN + PID


# ----------VERIFICACION------------

netstat -ano | more                  # revisar salida completa
tasklist | findstr <PID>             # identificar proceso por PID
```



# ----------PKTMON (ERROR INTERFAZ)------------

Explicación mínima: `pktmon` no usa nombres de interfaz directamente, requiere índice.

```bash
# LISTAR INTERFACES CON ÍNDICE
pktmon comp list                                              # ver interfaces disponibles con ID

# CAPTURA SIN FILTRO (TODAS LAS INTERFACES)
pktmon start --etw -p 0                                       # iniciar captura global

# CAPTURA POR INTERFAZ (USANDO ID)
pktmon filter remove                                          # limpiar filtros
pktmon filter add -i 12                                       # usar ifIndex (ej: 12 de Get-NetAdapter)
pktmon start --capture --file-name captura.etl                # iniciar captura filtrada

# DETENER CAPTURA
pktmon stop                                                   # detener captura

# CONVERTIR A PCAP
pktmon format captura.etl -o captura.pcapng                  # convertir para Wireshark

# VERIFICACIÓN
pktmon status                                                 # ver estado actual
```
# ----------POWERSHELL (CAPTURA TIPO TCPDUMP)------------

Explicación mínima: captura y análisis de tráfico nativo en Windows.

```bash
# LISTAR INTERFACES
Get-NetAdapter                                                # ver interfaces disponibles

# CAPTURA BÁSICA (PKTMON)
pktmon filter remove                                          # limpiar filtros
pktmon start --etw -p 0                                       # iniciar captura en tiempo real
pktmon stop                                                   # detener captura

# CAPTURA A ARCHIVO
pktmon start --capture --file-name captura.etl                # capturar tráfico a archivo
pktmon stop                                                   # detener captura

# CONVERTIR A PCAP (WIRESHARK)
pktmon format captura.etl -o captura.pcapng                   # convertir a formato Wireshark

# FILTRO POR PUERTO
pktmon filter add -p 80                                       # filtrar tráfico puerto 80
pktmon filter add -p 443                                      # filtrar HTTPS

# FILTRO POR IP
pktmon filter add -i 192.168.1.10                             # filtrar por IP

# VERIFICACIÓN
pktmon status                                                 # estado de captura
```

# ----------POWERSHELL (NETSH TRACE)------------

Explicación mínima: captura avanzada tipo sniffer integrada.

```bash
# INICIAR CAPTURA
netsh trace start capture=yes tracefile=c:\temp\trace.etl     # iniciar captura red

# DETENER CAPTURA
netsh trace stop                                              # detener captura

# CONVERTIR A PCAP
netsh trace convert c:\temp\trace.etl                         # convertir archivo

# VERIFICACIÓN
dir c:\temp                                                   # verificar archivo generado
```

# ----------POWERSHELL (MONITOREO CONEXIONES)------------

Explicación mínima: equivalente a netstat avanzado.

```bash
# CONEXIONES ACTIVAS
Get-NetTCPConnection                                          # ver conexiones TCP
Get-NetUDPEndpoint                                            # ver puertos UDP

# FILTRO POR PUERTO
Get-NetTCPConnection | Where-Object {$_.LocalPort -eq 80}     # filtrar puerto 80

# PROCESO ASOCIADO
Get-NetTCPConnection | Select-Object LocalAddress,LocalPort,State,OwningProcess   # ver PID

# MAPEAR PID A PROCESO
Get-Process -Id <PID>                                         # ver proceso

# MONITOREO EN TIEMPO REAL
while ($true) { Get-NetTCPConnection; Start-Sleep 2 }         # monitoreo continuo
```


# ----------CAPTURA EN VIVO TIPO TCPDUMP (WINDOWS)------------

```bash
# ----------INSTALAR TSHARK------------

winget install WiresharkFoundation.Wireshark   # instalar Wireshark + tshark


# ----------LISTAR INTERFACES------------

tshark -D                                      # ver interfaces disponibles


# ----------CAPTURA EN VIVO------------

tshark -i 1                                    # captura en tiempo real (equivalente tcpdump)
tshark -i 1 -n                                 # sin resolucion DNS (mejor rendimiento)


# ----------FILTROS (BPF)------------

tshark -i 1 -f "port 80"                       # trafico HTTP
tshark -i 1 -f "port 443"                      # trafico HTTPS
tshark -i 1 -f "host 192.168.1.10"             # filtrar por IP
tshark -i 1 -f "net 192.168.1.0/24"            # red completa


# ----------FILTROS AVANZADOS (DISPLAY)------------

tshark -i 1 -Y "http"                          # solo HTTP
tshark -i 1 -Y "dns"                           # solo DNS
tshark -i 1 -Y "tcp.port==443"                 # HTTPS (display filter)


# ----------VER CAMPOS CLAVE------------

tshark -i 1 -T fields -e ip.src -e ip.dst -e tcp.port   # solo IP origen/destino/puerto


# ----------GUARDAR CAPTURA------------

tshark -i 1 -w captura.pcapng                  # guardar captura
tshark -r captura.pcapng                       # leer archivo capturado


# ----------VERIFICACION------------

tshark -v                                      # verificar instalacion
```