####  Crear la zona inversa
```powershell
Add-DnsServerPrimaryZone -NetworkID "192.168.10.0/24" -ReplicationScope "Domain"
```

#### Crear 5 registros A
```powershell
Add-DnsServerResourceRecordA -Name "pc1" -ZoneName "midominio.local" -IPv4Address "192.168.10.10"
Add-DnsServerResourceRecordA -Name "pc2" -ZoneName "midominio.local" -IPv4Address "192.168.10.11"
Add-DnsServerResourceRecordA -Name "pc3" -ZoneName "midominio.local" -IPv4Address "192.168.10.12"
Add-DnsServerResourceRecordA -Name "pc4" -ZoneName "midominio.local" -IPv4Address "192.168.10.13"
Add-DnsServerResourceRecordA -Name "pc5" -ZoneName "midominio.local" -IPv4Address "192.168.10.14"
```

####  Crear Alias (CNAME)
```powershell
Add-DnsServerResourceRecordCName -Name "servidorweb" -ZoneName "midominio.local" -HostNameAlias "pc1.midominio.local"
Add-DnsServerResourceRecordCName -Name "archivos" -ZoneName "midominio.local" -HostNameAlias "pc2.midominio.local"
```

####  Crear los PTR en la zona inversa
```powershell
Add-DnsServerResourceRecordPtr -Name "10" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc1.midominio.local"
Add-DnsServerResourceRecordPtr -Name "11" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc2.midominio.local"
Add-DnsServerResourceRecordPtr -Name "12" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc3.midominio.local"
Add-DnsServerResourceRecordPtr -Name "13" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc4.midominio.local"
Add-DnsServerResourceRecordPtr -Name "14" -ZoneName "10.168.192.in-addr.arpa" -PtrDomainName "pc5.midominio.local"
```


