```bash
# ----------ADMIN BASICO------------
show running-config                    # ver configuracion actual
save config                            # guardar configuracion
show sys-info                          # ver version del sistema
show config                            # ver configuracion general
show interfaces                        # ver estado de interfaces
show run module vlan                   # ver config de VLAN por modulo
show vlan                              # listar VLANs
reset                                  # reiniciar switch
copy config tftp <ip> <file>           # backup a TFTP
copy tftp config <ip> <file>           # restaurar desde TFTP
restore factory-default                # restaurar a fabrica

# ----------RED------------
ip address switch <ip>                 # asignar IP al switch
ip default-gateway <ip>                # configurar gateway
ip route 0.0.0.0 0.0.0.0 <gw> 1        # ruta por defecto

# ----------VLAN BASICO------------
vlan configcontrol flexible            # modo flexible (recomendado)
vlan create <id> name <nombre> type port 1   # crear VLAN
vlan members add <vlan> <puerto>       # agregar puerto a VLAN
vlan members remove <vlan> <puerto>    # quitar puerto de VLAN
vlan ports <puertos> pvid <vlan>       # asignar VLAN nativa (access)

# ----------MODOS DE PUERTO------------
vlan ports <puerto> tagging tagAll     # trunk (todas etiquetadas)
vlan ports <puerto> tagging unTagAll   # access (sin etiquetas)
vlan ports <puerto> tagging tagPvidOnly    # solo PVID tagged
vlan ports <puerto> tagging untagPvidOnly  # solo PVID untagged

# ----------TRUNK EJEMPLO------------
configure terminal                     # entrar a configuracion
vlan create 10 name VLAN10 type port 1 # crear VLAN 10
vlan create 20 name VLAN20 type port 1 # crear VLAN 20
vlan create 30 name VLAN30 type port 1 # crear VLAN 30
vlan members add 10 1                  # agregar puerto 1 a VLAN 10
vlan members add 20 1                  # agregar puerto 1 a VLAN 20
vlan members add 30 1                  # agregar puerto 1 a VLAN 30
vlan ports 1 tagging tagAll            # configurar trunk en puerto 1

# ----------SEGURIDAD------------
passwd                                 # cambiar password admin
username <user> password <pass>         # crear usuario
ip ssh                                 # habilitar SSH
ip telnet                              # habilitar Telnet
no ip telnet                           # deshabilitar Telnet

# ----------WEB / GESTION------------
vlan mgmt <vlan>                       # definir VLAN de gestion
ipmgr web                              # configurar gestion web
web-server enable                      # habilitar web GUI

# ----------LOGS------------
telnet-access logging all              # habilitar logs de acceso
cli password telnet local              # auth local telnet

# ----------VERIFICACION------------
show vlan                              # validar VLANs
show interfaces                        # validar puertos
show ip route                          # validar rutas
```

# HABILITAR SNMP EN AVAYA
```bash
configure terminal                          # entrar a configuracion

snmp-server enable                          # habilitar servicio SNMP
snmp-server community public ro             # asegurar comunidad RO
snmp-server community private rw            # (opcional) RW

no snmp-server host 0.0.0.0                 # limpiar destinos invalidos
snmp-server host 172.40.60.4 v2c public # definir servidor SNMP correcto

save config                                 # guardar cambios

# ----------VERIFICACION SWITCH------------
show snmp-server                            # ver comunidades
show snmp-server host                       # ver destinos

# ----------PRUEBA CORRECTA (UDP)------------
nmap -sU -p 161 172.40.60.4               # escaneo SNMP UDP
snmpwalk -v2c -c public 172.40.60.4       # prueba real SNMP

# ----------SI SIGUE FALLANDO------------
ip access-list show                         # revisar ACL (si aplica)
show ip interface                           # validar IP activa
```
# SNMPV3 AVAYA ERS 
```bash
configure terminal                                                     # entrar en configuracion

snmp-server enable                                                     # habilitar SNMP

snmp-server name CORE-SW-01                                            # nombre del switch
snmp-server location "DC-RACK-02"                                      # ubicacion
snmp-server contact "noc-admin@empresa.local"                          # contacto administrador

snmp-server view MONITORVIEW 1 included                                # vista completa ISO
snmp-server view MONITORVIEW 1.3.6 included                            # vista internet mib

snmp-server user monitorusr sha aes read-view MONITORVIEW              # usuario SNMPv3 SHA/AES lectura
snmp-server user backupusr sha aes read-view snmpv1Objs                # usuario usando vista existente
snmp-server user auditorusr sha read-view MONITORVIEW                  # usuario SHA sin cifrado
snmp-server user nocv3 md5 des read-view MONITORVIEW                   # usuario MD5/DES

snmp-server host 10.55.100.25 v3 auth monitorusr                       # trap autenticado
snmp-server host 10.55.100.26 v3 auth-priv backupusr                   # trap autenticado y cifrado
snmp-server host 10.55.100.27 v3 auth nocv3 inform                     # inform SNMPv3
snmp-server host 192.168.88.50 v3 auth auditorusr                      # trap servidor monitoreo secundario

snmp-server notification-control linkDown enable                       # trap caida interfaz
snmp-server notification-control linkUp enable                         # trap subida interfaz
snmp-server notification-control coldStart enable                      # trap reinicio
snmp-server notification-control warmStart enable                      # trap reboot parcial

show snmp-server                                                       # verificar configuracion
show snmp-server user                                                  # verificar usuarios
show snmp-server host                                                  # verificar destinos
show snmp-server view                                                  # verificar vistas
```

# HOST SNMPV3
```bash
snmp-server host 172.18.10.15 v3 no-auth guestv3                      # trap sin autenticacion
snmp-server host 172.18.10.16 v3 auth securev3                        # trap autenticado
snmp-server host 172.18.10.17 v3 auth-priv securev3                   # trap autenticado+cifrado

snmp-server host 10.200.1.5 port 1162 v3 auth monitorusr              # puerto trap personalizado
snmp-server host 10.200.1.6 v3 auth monitorusr inform                 # usar INFORM
snmp-server host 10.200.1.7 v3 auth monitorusr inform retries 5       # retries INFORM
snmp-server host 10.200.1.8 v3 auth monitorusr inform timeout 300     # timeout INFORM
```
# USUARIOS SNMPV3
```bash
snmp-server user readonly sha read-view MONITORVIEW                   # SHA sin privacidad
snmp-server user readonly sha aes read-view MONITORVIEW               # SHA + AES
snmp-server user adminv3 md5 des read-view MONITORVIEW                # MD5 + DES
snmp-server user secops sha 3des read-view MONITORVIEW                # SHA + 3DES

snmp-server user secops sha aes read-view MONITORVIEW notify-view MONITORVIEW     # acceso traps
snmp-server user rwadmin sha aes read-view MONITORVIEW write-view MONITORVIEW      # acceso escritura
```
# PRUEBAS DESDE LINUX
```bash
snmpwalk -v3 -u monitor -l authPriv -a SHA -A '<AUTH_PASS>' -x AES -X '<PRIV_PASS>' 10.0.0.10 1.3.6.1.2.1.1   # prueba SNMPv3 completa

snmpget -v3 -u monitor -l authPriv -a SHA -A '<AUTH_PASS>' -x AES -X '<PRIV_PASS>' 10.0.0.10 sysName.0         # obtener hostname
snmpwalk -v3 -u monitor -l authNoPriv -a SHA -A '<AUTH_PASS>' 10.0.0.10 1.3.6.1.2.1.1                          # prueba sin cifrado
snmpwalk -v3 -u monitor -l noAuthNoPriv 10.0.0.10 1.3.6.1.2.1.1                                                 # prueba sin auth
```
