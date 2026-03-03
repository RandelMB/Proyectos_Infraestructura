


# Topologia

![[Pasted image 20260228194420.png]]

### Objetivo:
Conectar Mediante RDP tcp/3309 al Windows Server desde la wan a la lan

```sh
# Crear IP Virtual
config firewall vip
    edit "RDP"
        set src-filter "172.16.0.0/24"
        set service "RDP"
        set extip 172.16.0.251
        set mappedip "10.0.11.2"
        set extintf "VLAN11"
    next
end
```


![[Pasted image 20260228201630.png]]

### Configura la politica

```sh
# Configuramos la politica
    edit 4
        set name "Yo_TO_RDP"
        set srcintf "port1"
        set dstintf "VLAN11"
        set action accept
        set srcaddr "all"
        set dstaddr "RDP"
        set schedule "always"
        set service "RDP"
        set logtraffic all
    next
```

![[Pasted image 20260228201843.png]]