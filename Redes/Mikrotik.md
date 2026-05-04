





```sh
# Crear Action Logging
/system logging action add name=remote1514 target=remote remote=172.16.0.11 remote-port=1514 src-address=172.16.0.1
/system logging action add name=remote1515 target=remote remote=172.16.0.11 remote-port=1515 src-address=172.16.0.1
/system logging action add name=remote1516 target=remote remote=172.16.0.11 remote-port=1516 src-address=172.16.0.1
/system logging action add name=remote1517 target=remote remote=172.16.0.11 remote-port=1517 src-address=172.16.0.1
/system logging action add name=remote1518 target=remote remote=172.16.0.11 remote-port=1518 src-address=172.16.0.1

```










## Configuración IPsec (modo compatible con Shrew)

```sh
# Profile IKEv2 (tu propio profile)
/ip ipsec profile add name=profile-work hash-algorithm=sha256 enc-algorithm=aes-256 dh-group=ecp256 dpd-interval=30s dpd-maximum-failures=5


# Peer (IKEv2)
/ip ipsec peer add name=peer-work address=0.0.0.0/0 exchange-mode=ike2 profile=profile-work passive=yes

# Mode Config (pool para clientes)
/ip ipsec mode-config add name=cfg-work address-pool=vpn-pool address-prefix-length=24 system-dns=yes

# Policy Group
/ip ipsec policy group add name=group-work

# Policy Template
/ip ipsec policy add group=group-work template=yes src-address=0.0.0.0/0 dst-address=172.16.0.0/24 proposal=proposal-work

# Identity (PSK o usuario)

# Opción PSK simple
/ip ipsec identity add peer=peer-work auth-method=pre-shared-key secret=MiPSK generate-policy=port-strict mode-config=cfg-work policy-template-group=group-work

# Firewall
/ip firewall filter add chain=input protocol=udp port=500,4500 action=accept  
/ip firewall filter add chain=input protocol=ipsec-esp action=accept

# NAT bypass (clave)
/ip firewall nat add chain=srcnat src-address=172.16.0.0/24 dst-address=172.16.0.0/24 action=accept
/ip firewall nat add chain=srcnat src-address=192.168.2.0/24 dst-address=172.16.0.0/24 action=accept


```