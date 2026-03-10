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



