
![[Pasted image 20260309091009.png]]

```c#
//# Indentificar el hostname completo del Server
hostname

//# Autorizar DHCP con el FQDN correcto
Add-DhcpServerInDC -DnsName bernardo.bernardo-server.local -IPAddress 192.168.0.10

//# Validar la autorizacion
Get-DhcpServerInDC
```

### Flujo DHCP
1. **DHCPDISCOVER** – el cliente busca servidor    
2. **DHCPOFFER** – el servidor ofrece una IP    
3. **DHCPREQUEST** – el cliente solicita esa IP    
4. **DHCPACK** – el servidor confirma la asignación

```sh
# Reservacion de DHCP
Add-DhcpServerv4Reservation `
-ScopeId 192.168.20.0 `
-IPAddress 192.168.20.50 `
-ClientId "D4-A2-CD-97-62-32" `
-Name "F-DPC-05.domini.com.do" `
-Description "Pedro Pablo"
```