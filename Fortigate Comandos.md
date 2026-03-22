



## Rutas

```
# Ver Rutas
get router info routing-table all

# Validar por donde estamos saliendo
get router info routing-table details 10.0.11.1



config router static
    edit 2
        set dst 10.0.11.0 255.255.255.0
        set device "port5"
        set gateway 190.190.190.2
    next
end

config router static
    edit 2
        set dst 192.168.0.0 255.255.255.0
        set device "port5"
        set gateway 190.190.190.1
    next
end
```



###  IPsec Tunel
```Python
# Diagnostico Troubleshooting
diagnose sniffer packet any 'udp and (port 500 or port 4500)' 4	Captura de paquetes
get vpn ipsec tunnel summary	
diagnose vpn ike gateway list	
diagnose vpn tunnel list	
	
diagnose debug enable	Habilitar logs en vivo
diagnose debug application ike -1	Habilitar logs en vivo
diagnose debug disable	Parar logs en vivo

```

### Snifing (Captura de paquetes)

```
diagnose sniffer packet any 'host 172.16.0.242 and port 500' 4
```

### Firewall Rule


### Virtual IP (PortFoward)
```c

    edit "RDP_TO_FILE"
        set extip 172.16.0.241
        set mappedip "10.0.2.100"
        set extintf "port1"
        set portforward enable
        set extport 6001
        set mappedport 3389
    next
end

## -----Agregarlo a politica---------#
config firewall policy
    edit 4
        set name "Yo_TO_RDP"
        set srcintf "port1"
        set dstintf "VLAN11"
        set action accept
        set srcaddr "all"
        set dstaddr "RDP_TO_AD_DS" "RDP_TO_FILE" 
        set schedule "always"
        set service "RDP"
        set logtraffic all
        set nat enable
    next
end
```