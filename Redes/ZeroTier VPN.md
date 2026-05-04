

```sh
# 1. INSTALAR ZEROTIER (WSL - Debian/Ubuntu)
curl -s https://install.zerotier.com | sudo bash   # instala zerotier
sudo mkdir -p /var/run/zerotier                    # crea directorio del socket
sudo zerotier-one -d                               # inicia el daemon manualmente (WSL no usa systemd)

zerotier-cli info                                  # debe mostrar estado ONLINE

# 6. AUTORIZAR (ESTO SE HACE EN WEB)
# https://my.zerotier.com
# -> entrar a la red
# -> marcar [✔] Auth al nodo (MAC: 16:41:e0:94:45:b3)


zerotier-cli join ID_NETWORK                       # te unes al network (usa tu Network ID)
zerotier-cli listnetworks                          # verifica si dice ACCESS_DENIED o OK

zerotier-cli leave {network_name}                # salir de la red
zerotier-cli join {network_name}                 # volver a entrar
ip a                                               # buscar interfaz ztXX  (IP tipo 10.x.x.x)


ssh user@10.x.x.x                               # conectarse al WSL usando IP de ZeroTier
```

```
Script
```