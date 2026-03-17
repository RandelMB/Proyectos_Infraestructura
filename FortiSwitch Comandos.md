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
# Ver estado de puertos
get switch interface
get switch interface status
get switch interface summary
get switch physical-port

# Configurar puerto
config switch interface
edit <port>
set description "Puerto"
set speed auto
set duplex auto
set status up
next
end

# Asignar puertos a VLAN
config switch interface
edit <port>
set native-vlan <vlan_id>
set allowed-vlans <vlan_id>
next
end

# Deshabilitar / habilitar
config switch interface
edit <port>
set status down
next
end

# Estadísticas
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

Si quieres, te hago otro bloque igual pero de **FortiGate (más útil si estás usando FortiLink con el switch)** o uno más enfocado a **troubleshooting real (lo que de verdad se usa en producción)**.
