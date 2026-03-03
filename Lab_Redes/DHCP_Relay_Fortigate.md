

# Topologia

![[Pasted image 20260301074715.png]]

### Puerto troncal del Core2
```python
CORE-02(config-if)#do sh ru int Ethernet11/3
Building configuration...

Current configuration : 174 bytes
!
interface Ethernet11/3
 description To-Fortigate
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 2,6,11,12,30
 switchport mode trunk
 duplex auto
end
```

### Puerto Trunk al SWACC-002
```Python
CORE-02(config-if)#do sh ru int Ethernet11/2
Building configuration...

Current configuration : 174 bytes
!
interface Ethernet11/2
 description To_SWACC-002
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 2,6,11,12,30
 switchport mode trunk
 duplex auto
end
```

### En mi caso cree una interface redundante para futuras pruebas con LACP

```python
    edit "Local Red"
        set vdom "root"
        set type redundant
        set member "port9" "port10"
        set alias "Local Red"
        set device-identification enable
        set lldp-reception disable
        set lldp-transmission disable
        set role lan
        set snmp-index 15
    next
```

### Crear las vlans en Fortigarte especificando el **"set dhcp-relay-service enable" y "set dhcp-relay-ip "10.0.11.2"**
```python
config system interface
    edit "VLAN2"
        set vdom "root"
        set dhcp-relay-service enable
        set ip 10.0.2.10 255.255.255.0
        set allowaccess ping https ssh snmp fgfm ftm
        set alias "VLAN2"
        set role lan
        set snmp-index 15
        set dhcp-relay-ip "10.0.11.2"
        set interface "Local Red"
        set vlanid 2
    next
    edit "VLAN11"
        set vdom "root"
        set ip 10.0.11.10 255.255.255.0
        set allowaccess ping https ssh snmp fgfm ftm
        set alias "VLAN11"
        set device-identification enable
        set role lan
        set snmp-index 17
        set dhcp-relay-ip "10.0.11.2"
        set interface "Local Red"
        set vlanid 11
    next
    edit "VLAN30"
        set vdom "root"
        set ip 10.0.30.10 255.255.255.0
        set allowaccess ping https ssh snmp 
        set alias "ADMINISTRACION"
        set device-identification enable
        set role lan
        set snmp-index 18
        set dhcp-relay-ip "10.0.11.2"
        set interface "Local Red"
        set vlanid 30
    next
    edit "VLAN6"
        set vdom "root"
        set ip 10.0.6.10 255.255.255.0
        set allowaccess ping https ssh snmp fgfm ftm
        set alias "TECNOLOGIA"
        set device-identification enable
        set role lan
        set snmp-index 19
        set dhcp-relay-ip "10.0.11.2"
        set interface "Local Red"
        set vlanid 6
    next
    edit "VLAN12"
        set vdom "root"
        set ip 10.0.12.10 255.255.255.0
        set allowaccess ping https ssh snmp fgfm ftm
        set alias "OPERACIONES"
        set device-identification enable
        set role lan
        set snmp-index 20
        set dhcp-relay-ip "10.0.11.2"
        set interface "Local Red"
        set vlanid 12 
    next
end

# Para ver las vlans
show | grep -f vlan
```

# Despues en el Servidor dhcp creas Ambitos ipv4

![[Pasted image 20260301082513.png]]



```python
#Crear Ambito DHCP
Add-DhcpServerv4Scope -Name "VLAN6-TECNOLOGIA" -StartRange 10.0.6.150 -EndRange 10.0.6.200 -SubnetMask 255.255.255.0 -State Active

# Exclusiones 
Add-DhcpServerv4ExclusionRange -ScopeId 10.0.6.0 -StartRange 10.0.6.1 -EndRange 10.0.6.149

# Configurar gateway por defecto y DNS
Set-DhcpServerv4OptionValue -ScopeId 10.0.6.0 -Router 10.0.6.10 -DnsDomain "cybersupports.net" -DnsServer 10.0.11.2

# Tiempo de lease (por defecto 8 días)
Set-DhcpServerv4Scope -ScopeId 10.0.6.0 -LeaseDuration (New-TimeSpan -Days 7)
```

```python
Add-DhcpServerv4Scope -Name "VLAN12-OPERACIONES" -StartRange 10.0.12.150 -EndRange 10.0.12.200 -SubnetMask 255.255.255.0 -State Active

# Exclusiones
Add-DhcpServerv4ExclusionRange -ScopeId 10.0.12.0 -StartRange 10.0.12.1 -EndRange 10.0.12.149

# Gateway y DNS
Set-DhcpServerv4OptionValue -ScopeId 10.0.12.0 -Router 10.0.12.10 -DnsDomain "cybersupports.net" -DnsServer 10.0.11.2

# Lease 7 días
Set-DhcpServerv4Scope -ScopeId 10.0.12.0 -LeaseDuration (New-TimeSpan -Days 7)
```

```python
Add-DhcpServerv4Scope -Name "VLAN30-ADMINISTRACION" -StartRange 10.0.30.150 -EndRange 10.0.30.200 -SubnetMask 255.255.255.0 -State Active

# Exclusiones
Add-DhcpServerv4ExclusionRange -ScopeId 10.0.30.0 -StartRange 10.0.30.1 -EndRange 10.0.30.149

# Gateway y DNS
Set-DhcpServerv4OptionValue -ScopeId 10.0.30.0 -Router 10.0.30.10 -DnsDomain "cybersupports.net" -DnsServer 10.0.11.2

# Lease 7 días
Set-DhcpServerv4Scope -ScopeId 10.0.30.0 -LeaseDuration (New-TimeSpan -Days 7)
```

![[Pasted image 20260301105450.png]]
![[Pasted image 20260301084627.png]]

# configuramos modo acces con las vlan correspondientes

```
!
interface Ethernet0/0
 description PC_Windows
 switchport access vlan 2
 switchport mode access
 duplex auto
!
interface Ethernet0/1
 switchport access vlan 30
 switchport mode access
 duplex auto
!
interface Ethernet0/2
 switchport access vlan 12
 switchport mode access
 duplex auto
!
interface Ethernet0/3
 switchport access vlan 6
 switchport mode access
 duplex auto
!
```

![[Pasted image 20260301084843.png]]
