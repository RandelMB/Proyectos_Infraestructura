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

# HABILITAR SNMP EN AVAYA------------
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