## Link Layer Discovery Protocol
```cs
get switch lldp neighbors
get switch lldp auto-isl-status      # Auto-inter-switch-LAG statistic and status.
get switch lldp neighbors-detail     # LLDP neighbor details.
get switch lldp neighbors-summary    # LLDP neighbor summary.
get switch lldp profile              # LLDP configuration profiles.
get switch lldp settings             # Global LLDP configuration.
get switch lldp stats                # LLDP port statistics.
```

## Sniffer
```sh
diagnose netlink interface list                       # Nombre de las interfaces
diagnose sniffer packet port_1 'port 80' 4            # Caputa del puerto 80
diagnose sniffer packet port_14 '' 4                  # 
```
## VLAN

```cs
# Ver VLANs
get switch vlan
get switch vlan summary
get switch vlan list

# Configurar VLAN
config switch vlan
edit <vlan_id>
set description "Nombre VLAN"
set ip <ip> <mask>
set vlanid <vlan_id>
next
end

# Borrar VLAN
config switch vlan
delete <vlan_id>
end
```

## Interfaces / Puertos

```cs
//# Ver estado de puertos
get switch interface
get switch interface status
get switch interface summary
get switch physical-port

//# Configurar puerto
config switch interface
edit <port>
set description "Puerto"
set speed auto
set duplex auto
set status up
next
end

//# Asignar puertos a VLAN
config switch interface
edit <port>
set native-vlan <vlan_id>
set allowed-vlans <vlan_id>
next
end

//# Deshabilitar / habilitar
config switch interface
edit <port>
set status down
next
end

//# Estadísticas
get switch interface stats
diagnose switch physical-port stats
```

## PoE (Power over Ethernet)

```cs
# Estado PoE
get switch poe
get switch poe status
get switch poe detail

# Configurar PoE
config switch interface
edit <port>
set poe-status enable
set poe-limit <watts>
next
end

# Reiniciar PoE
execute switch poe-reset <port>
```

## STP (Spanning Tree Protocol)

```cs
# Ver STP
get switch stp settings
get switch stp instance
get switch stp ports

# Configurar STP
config switch stp settings
set mode rstp
set priority <value>
end

# Configurar puerto STP
config switch interface
edit <port>
set stp-state enabled
set edge-port enable
next
end
```

## LACP / Trunk (LAG)

```cs
# Ver LAG
get switch trunk
get switch trunk status
get switch trunk members

# Crear LAG
config switch trunk
edit <name>
set mode lacp-active
set members <port1> <port2>
next
end

# Borrar LAG
config switch trunk
delete <name>
end
```

## Seguridad de Puertos

```cs
# Ver seguridad
get switch interface-security
get switch mac-address-table

# Configurar seguridad
config switch interface
edit <port>
set port-security enable
set max-mac-count <num>
next
end

# Ver MACs
diagnose switch mac-address list
```

## DHCP Snooping

```cs
# Ver DHCP Snooping
get switch dhcp-snooping
get switch dhcp-snooping status

# Configurar DHCP Snooping
config switch dhcp-snooping
set status enable
set vlan <vlan_id>
end

# Puerto confiable
config switch interface
edit <port>
set dhcp-snooping trusted
next
end
```

## IGMP Snooping

```cs
# Ver IGMP
get switch igmp-snooping
get switch igmp-snooping groups

# Configurar IGMP
config switch igmp-snooping
set status enable
set vlan <vlan_id>
end
```

## QoS (Quality of Service)

```cs
# Ver QoS
get switch qos
get switch qos policy

# Configurar QoS
config switch qos policy
edit <policy_name>
set priority high
set bandwidth <kbps>
next
end

# Aplicar QoS a puerto
config switch interface
edit <port>
set qos-policy <policy_name>
next
end
```

## SNMP

```cs
# Ver SNMP
get system snmp sysinfo
get system snmp community

# Configurar SNMP
config system snmp community
edit 1
set name "public"
set query-v1-status enable
set query-v2c-status enable
next
end
```

## Logs / Diagnóstico

```cs
# Ver logs
execute log display
execute log filter category <category>

# Diagnóstico general
diagnose debug enable
diagnose debug console timestamp enable

# Ver errores
diagnose debug crashlog read
```

## Firmware / Sistema

```cs
# Información del sistema
get system status
get system performance
get system uptime

# Reiniciar
execute reboot

# Backup configuración
execute backup config tftp <file> <server_ip>

# Restaurar
execute restore config tftp <file> <server_ip>
```

## MAC Address Table

```cs
# Ver tabla MAC
get switch mac-address-table
get switch mac-address-table dynamic
get switch mac-address-table static

# Limpiar tabla
execute switch clear mac-address-table
```


# HABILITAR VLAN DE ADMINISTRACION HACIA CPU/INTERNAL
```bash
config switch interface

edit internal
set allowed-vlans 200      # permitir VLAN administración hacia CPU
next

end
```
## CREAR INTERFAZ L3/SVI DE ADMINISTRACION
```bash
config system interface

edit "Admininstracion"
set ip 172.25.200.45 255.255.255.0
set vlanid 200
set interface "internal"
set allowaccess ping https ssh snmp
set alias "SW-CLUSTER-FIREWALL"

next
end
```
## CONFIGURAR RUTA DEFAULT
```bash
config router static

edit 1
set dst 0.0.0.0 0.0.0.0
set gateway 172.25.200.1
set device "Admininstracion"

next
end
```
## HABILITAR ACCESO ADMINISTRATIVO
```bash
config system interface

edit "Admininstracion"
set allowaccess ping https http ssh snmp

next
end
```
## VALIDACION
```bash
show switch interface
show system interface
get router info routing-table all
execute ping 172.25.200.1
```

# CONFIGURAR USUARIO SNMPv3 EN FORTISWITCH
```bash
config system snmp user

edit "1"

set security-level auth-priv      # autenticación + cifrado
set auth-proto sha                # algoritmo autenticación
set auth-pwd CLAVE-AUTH           # password auth
set priv-proto aes                # cifrado AES
set priv-pwd CLAVE-PRIV           # password cifrado

set queries enable                # permitir consultas SNMP
set query-port 161                # puerto SNMP

next
end
```
## CONFIGURAR HOST DESTINO PARA TRAPS
```bash
config system snmp user

edit "1"

set notify-hosts 172.25.200.40   # servidor SNMP/Observium/Zabbix
set events cpu-high mem-low log-full intf-ip

next
end
```
## HABILITAR SNMP EN INTERFAZ ADMIN
```bash
config system interface

edit "Admininstracion"
set allowaccess ping https ssh snmp

next
end
```
## VALIDAR CONFIGURACION
```bash
show system snmp user
show system interface
```
## PROBAR DESDE LINUX / OBSERVIUM
```bash
snmpwalk -v3 -u 1 \
-l authPriv \
-a SHA -A CLAVE-AUTH \
-x AES -X CLAVE-PRIV \
172.25.200.45
```