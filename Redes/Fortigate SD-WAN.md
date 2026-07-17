# SD-WAN BASICO
```bash
config system interface
    edit "wan1"
        set alias "ISP1"
        set mode dhcp
        set role wan
        set distance 10
    next
    edit "wan2"
        set alias "ISP2"
        set ip 192.168.20.2 255.255.255.0
        set role wan
        set distance 20
    next
end                                                            # configurar interfaces WAN

config system sdwan
    set status enable                                          # habilitar SD-WAN

    config members
        edit 1
            set interface "wan1"
            set gateway 192.168.1.1
            set priority 1
            set cost 10
        next
        edit 2
            set interface "wan2"
            set gateway 192.168.20.1
            set priority 2
            set cost 20
        next
    end
end                                                            # agregar miembros SD-WAN

config router static
    edit 1
        set dst 0.0.0.0/0
        set sdwan enable
    next
end                                                            # ruta default SD-WAN

get system sdwan status                                        # estado SD-WAN
diagnose sys sdwan member                                      # ver miembros
diagnose sys sdwan health-check                                # ver SLA
```

Fuente oficial Fortinet: ([Biblioteca de Documentos de Fortinet](https://docs2.fortinet.com/document/fortigate/6.4.8/administration-guide/256518/configuring-sd-wan-in-the-cli?utm_source=chatgpt.com "Configuring SD-WAN in the CLI | FortiGate / FortiOS 6.4.0 | Fortinet Document Library"))

# SD-WAN CON ZONAS

```bash
config system sdwan
    set zone-mode enable                                       # habilitar zonas

    config zone
        edit "INTERNET"
        next

        edit "VPN"
        next

        edit "MPLS"
        next
    end

    config members
        edit 1
            set interface "wan1"
            set zone "INTERNET"
        next

        edit 2
            set interface "wan2"
            set zone "INTERNET"
        next

        edit 3
            set interface "vpn-hub1"
            set zone "VPN"
        next

        edit 4
            set interface "mpls1"
            set zone "MPLS"
        next
    end
end                                                            # asignar miembros a zonas

show system sdwan                                              # mostrar configuración
```

Fuente oficial Fortinet: ([Biblioteca de Documentos Fortinet](https://docs.fortinet.com/document/fortigate/7.4.3/cli-reference/838040159/config-system-sdwan?utm_source=chatgpt.com "config system sdwan | FortiGate / FortiOS 7.4.3 | Fortinet Document Library"))

# PERFORMANCE SLA
```bash
config system sdwan
    config health-check
        edit "Google-DNS"
            set server "8.8.8.8"
            set protocol ping
            set interval 500
            set failtime 5
            set recoverytime 5
            set members 1 2
            set update-static-route enable
            set sla-fail-log-period 10
            set sla-pass-log-period 10

            config sla
                edit 1
                    set latency-threshold 100
                    set jitter-threshold 30
                    set packetloss-threshold 5
                next
            end
        next

        edit "Cloudflare"
            set server "1.1.1.1"
            set protocol ping
            set members 1 2
        next
    end
end                                                            # monitoreo SLA

diagnose sys sdwan health-check                                # verificar SLA
```

Fuente oficial Fortinet: ([Biblioteca de Documentos de Fortinet](https://docs2.fortinet.com/document/fortigate/6.4.8/administration-guide/256518/configuring-sd-wan-in-the-cli?utm_source=chatgpt.com "Configuring SD-WAN in the CLI | FortiGate / FortiOS 6.4.0 | Fortinet Document Library"))

# REGLAS SD-WAN
```bash
config system sdwan
    config service
        edit 1
            set name "VoIP"
            set mode sla
            set dst "all"
            set src "all"
            set priority-members 1 2

            config sla
                edit "Google-DNS"
                    set id 1
                next
            end
        next

        edit 2
            set name "Web"
            set mode load-balance
            set src "all"
            set dst "all"
            set priority-members 1 2
        next

        edit 3
            set name "Critical"
            set mode priority
            set priority-members 1 2
        next

    end
end                                                            # reglas tráfico SD-WAN

diagnose sys sdwan service                                     # ver reglas
```

Fuente oficial Fortinet: ([Biblioteca de Documentos de Fortinet](https://docs2.fortinet.com/document/fortigate/6.4.7/administration-guide/380145/configuring-sd-wan-rules?utm_source=chatgpt.com "Configuring SD-WAN rules | FortiGate / FortiOS 6.4.0 | Fortinet Document Library"))

# MODOS DE BALANCEO

```bash
config system sdwan
    config service
        edit 1
            set mode manual                                    # manual
        next

        edit 2
            set mode priority                                  # prioridad
        next

        edit 3
            set mode sla                                       # SLA
        next

        edit 4
            set mode load-balance                              # balanceo
        next

        edit 5
            set mode source-ip-based                           # hash source-ip
        next

        edit 6
            set mode volume                                    # volumen
        next

    end
end
```


# ALGORITMOS Y TIE-BREAK
```bash
config system sdwan
    set load-balance-mode source-ip-based                      # algoritmo hash

    config service
        edit 1
            set tie-break zone                                 # por zona
        next

        edit 2
            set tie-break cfg-order                            # orden config
        next

        edit 3
            set tie-break fib-best-match                       # mejor ruta
        next

        edit 4
            set tie-break input-device                         # interfaz entrada
        next
    end
end                                                            # desempate SD-WAN
```


# SD-WAN + BGP
```bash
config system sdwan
    config neighbor
        edit "10.10.10.1"
            set member 1 2
            set health-check "Google-DNS"
            set sla-id 1
            set minimum-sla-meet-members 1
        next
    end
end                                                            # asociar SLA a BGP

config router bgp
    set as 65001

    config neighbor
        edit "10.10.10.1"
            set remote-as 65002
            set soft-reconfiguration enable
        next
    end
end                                                            # configurar BGP

get router info bgp summary                                    # verificar BGP
```


# SD-WAN + ADVPN
```bash
config system sdwan
    config zone
        edit "OVERLAY"
            set advpn-select enable
            set advpn-health-check "Google-DNS"
            set service-sla-tie-break fib-best-match
        next
    end
end                                                            # habilitar ADVPN-aware

config vpn ipsec phase1-interface
    edit "advpn-hub1"
        set interface "wan1"
        set ike-version 2
        set net-device disable
        set add-route disable
        set dpd on-idle
        set auto-discovery-sender enable
    next
end                                                            # túnel ADVPN

diagnose vpn tunnel list                                       # verificar túneles
```


# SD-WAN DUPLICATION
```bash
config system sdwan
    config duplication
        edit 1
            set service-id 1
            set packet-duplication force
            set packet-de-duplication enable
            set srcaddr "all"
            set dstaddr "all"
        next
    end
end                                                            # duplicación tráfico crítico

show system sdwan                                              # validar
```

# POLITICAS FIREWALL SD-WAN
```bash
config firewall policy
    edit 1
        set name "LAN_to_SDWAN"
        set srcintf "lan"
        set dstintf "virtual-wan-link"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
        set logtraffic all
    next
end                                                            # política SD-WAN

show firewall policy                                           # verificar políticas
```

# DIAGNOSTICO Y TROUBLESHOOTING
```bash
diagnose sys sdwan health-check                                # SLA status
diagnose sys sdwan member                                      # miembros
diagnose sys sdwan service                                     # reglas
diagnose firewall proute list                                  # policy routes
diagnose netlink interface list                                # interfaces
diagnose vpn tunnel list                                       # túneles VPN
get router info routing-table all                              # tabla rutas
get system performance status                                  # estado sistema
execute ping 8.8.8.8                                           # prueba conectividad
execute traceroute 8.8.8.8                                     # prueba ruta
```

# CONFIGURACIONES AVANZADAS
```bash
config system sdwan
    set app-perf-log-period 300                                # logs SLA

    config service
        edit 1
            set passive-measurement enable                     # medición pasiva
            set shortcut enable                                # usar shortcut
            set use-shortcut-sla enable                        # SLA shortcut
            set packet-loss-threshold 10
            set latency-threshold 100
            set jitter-threshold 20
        next
    end
end                                                            # opciones avanzadas
```

# ESTADO GENERAL SD-WAN
```bash
get system sdwan status                                        # estado general SD-WAN
show system sdwan                                              # configuración completa
diagnose sys sdwan member                                      # miembros SD-WAN
diagnose sys sdwan service                                     # reglas SD-WAN
diagnose sys sdwan health-check                                # SLA y probes
diagnose sys sdwan neighbor                                    # vecinos SD-WAN
diagnose sys sdwan zone                                        # zonas SD-WAN
diagnose sys sdwan intf-health-check                           # salud interfaces
```

# SLA / HEALTH-CHECK
```bash
diagnose sys sdwan health-check                                # SLA completo
diagnose sys sdwan health-check list                           # lista SLA
diagnose sys sdwan health-check status                         # estado SLA
diagnose sys sdwan health-check summary                        # resumen SLA
diagnose sys sdwan health-check-history                        # historial SLA
diagnose sys sdwan probe-status                                # probes activos
diagnose sys sdwan intf-sla-log                                # logs SLA interfaz
```

# TRAFICO Y SELECCION DE RUTA
```bash
diagnose sys sdwan service                                     # reglas y selección
diagnose sys sdwan route-tag                                   # tags de rutas
diagnose firewall proute list                                  # policy routes
get router info routing-table all                              # tabla de rutas
get router info kernel                                         # rutas kernel
diagnose ip route list                                         # rutas activas
diagnose netlink route list                                    # rutas netlink
```

# DEBUG FLOW SD-WAN
```bash
diagnose debug reset                                           # limpiar debug
diagnose debug console timestamp enable                        # timestamps
diagnose debug flow filter addr 8.8.8.8                        # filtrar IP
diagnose debug flow filter proto 6                             # TCP
diagnose debug flow filter dport 443                           # puerto destino
diagnose debug flow show function-name enable                  # mostrar funciones
diagnose debug flow trace start 100                            # iniciar captura
diagnose debug enable                                          # habilitar debug
```

# DETENER DEBUG
```bash
diagnose debug disable                                         # detener debug
diagnose debug flow trace stop                                 # parar trace
diagnose debug reset                                           # limpiar filtros
```

# MONITOREO EN TIEMPO REAL
```bash
diagnose sys top                                               # procesos realtime
diagnose hardware deviceinfo nic wan1                          # estado NIC wan1
diagnose hardware deviceinfo nic wan2                          # estado NIC wan2
diagnose netlink interface list                                # interfaces activas
get system performance status                                  # CPU/RAM/sesiones
diagnose sys session stat                                      # estadísticas sesiones
diagnose sys session list                                      # sesiones activas
```

# PRUEBAS DE CONECTIVIDAD
```bash
execute ping 8.8.8.8                                           # ping básico
execute ping-options source 192.168.1.2                        # origen ping
execute ping-options interface wan1                            # usar WAN1
execute ping-options interface wan2                            # usar WAN2
execute ping-options repeat-count 10                           # repetir pruebas
execute traceroute 8.8.8.8                                     # traceroute
execute telnet 8.8.8.8 53                                      # probar puerto
```

# CAPTURA DE PAQUETES
```bash
diagnose sniffer packet any "host 8.8.8.8" 4                  # captura simple
diagnose sniffer packet wan1 "host 8.8.8.8" 4                 # captura WAN1
diagnose sniffer packet wan2 "host 8.8.8.8" 4                 # captura WAN2
diagnose sniffer packet any "icmp" 4                          # ICMP
diagnose sniffer packet any "port 443" 4                      # HTTPS
diagnose sniffer packet any "host 1.1.1.1 and icmp" 6         # verbose
```

# VPN + SD-WAN

```bash
diagnose vpn tunnel list                                       # túneles VPN
diagnose vpn ike gateway list                                  # gateways IKE
diagnose vpn ike log-filter clear                              # limpiar filtro
diagnose vpn ike log-filter dst-addr4 1.1.1.1                 # filtrar peer
diagnose debug application ike -1                              # debug IKE
diagnose debug enable                                          # habilitar debug
```

# SD-WAN + BGP

```bash
get router info bgp summary                                    # resumen BGP
get router info bgp neighbors                                  # vecinos BGP
get router info routing-table bgp                              # rutas BGP
diagnose ip router bgp all                                     # debug BGP
diagnose sys sdwan neighbor                                    # vecinos SD-WAN
```

# LOGS Y EVENTOS

```bash
execute log filter category 0                                  # traffic logs
execute log filter field subtype sdwan                         # filtrar SD-WAN
execute log display                                            # mostrar logs
diagnose log test                                              # probar logging
diagnose debug application wad -1                              # debug proxy/WAD
```

# VERIFICACION RAPIDA COMPLETA

```bash
get system sdwan status                                        # estado global
diagnose sys sdwan health-check                                # SLA
diagnose sys sdwan service                                     # reglas
get router info routing-table all                              # rutas
diagnose firewall proute list                                  # policy routes
execute ping 8.8.8.8                                           # conectividad
diagnose sniffer packet any "host 8.8.8.8" 4                  # captura
get system performance status                                  # rendimiento
```