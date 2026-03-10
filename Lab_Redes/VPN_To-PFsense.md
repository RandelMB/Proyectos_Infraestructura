

Ejemplo básico de **VPN IPsec site-to-site en CLI** en un **FortiGate**.  
Variables de ejemplo:

- Interfaz WAN: `port1`
- IP pública remota: `1.1.1.1`
- Red local: `192.168.1.0/24`
- Red remota: `192.168.2.0/24`
- PSK: `ClaveVPN123`
    

---

# 1. Phase1 (IKE)

```
config vpn ipsec phase1-interface
    edit "Pfsense-P1"
        set interface "port1"
        set ike-version 2
        set peertype any
        set remote-gw 172.16.0.244
        set proposal des-sha256
        set dhgrp 14
        set nattraversal enable
        set dpd on-idle
        set psksecret Admin123
    next
end


# 2. Phase2 (IPsec)


config vpn ipsec phase2-interface
    edit "Pfsense-P2"
        set phase1name "Pfsense-P1"
        set proposal des-sha256
        set dhgrp 14
        set src-subnet 172.16.5.0 255.255.255.0
        set dst-subnet 172.16.4.0 255.255.255.0
    next
end
```

---

# 3. Rutas estáticas

```
config router static
    edit 1
        set dst 192.168.2.0 255.255.255.0
        set device "VPN-SITE"
    next
end
```

---

# 4. Políticas de firewall

```
config firewall policy
    edit 1
        set name "LAN_to_VPN"
        set srcintf "internal"
        set dstintf "VPN-SITE"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat disable
    next

    edit 2
        set name "VPN_to_LAN"
        set srcintf "VPN-SITE"
        set dstintf "internal"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat disable
    next
end
```

---

# 5. Verificar VPN

```
get vpn ipsec tunnel summary
diagnose vpn tunnel list
```

---

Si quieres, puedo darte también:

- **plantilla genérica para copiar y solo cambiar IPs**
    
- **ejemplo FortiGate ↔ FortiGate**
    
- **ejemplo FortiGate ↔ Cisco / Mikrotik / Palo Alto**.