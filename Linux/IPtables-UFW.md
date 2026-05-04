# UFW
```bash
# UFW CONFIGURACION BASE
sudo ufw reset                          # limpiar reglas
sudo ufw default deny incoming          # denegar entrada por defecto
sudo ufw default allow outgoing         # permitir salida por defecto
sudo ufw enable                         # habilitar firewall

# -------- PUERTOS NECESARIOS -----------
sudo ufw allow 22/tcp                   # ssh
sudo ufw allow 80/tcp                   # http
sudo ufw allow 443/tcp                  # https
sudo ufw allow 53                       # dns tcp/udp
sudo ufw allow 1194/udp                 # openvpn

# -------- SALIDA CONTROLADA -----------
sudo ufw allow out 53                   # dns salida
sudo ufw allow out 80/tcp               # http salida
sudo ufw allow out 443/tcp              # https salida
sudo ufw allow out 123/udp              # ntp
sudo ufw allow out proto icmp           # ping salida

# -------- INTERFACES ESPECIFICAS -----------
sudo ufw allow in on eth0 to any port 80 proto tcp     # http en eth0
sudo ufw allow in on eth0 to any port 443 proto tcp    # https en eth0
sudo ufw deny in on eth0 to any port 80 proto tcp      # bloquear http en eth0

# -------- VERIFICACION -----------
sudo ufw status verbose                # estado y reglas
sudo ufw numbered                      # reglas numeradas
```


# Iptables

| Cadena                       | Tipo de tabla | Función principal                                                             | Nota / Alcance                                                          |
| ---------------------------- | ------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **INPUT**                    | filter        | Controla paquetes **entrantes** al host                                       | Normal, estándar de Linux                                               |
| **OUTPUT**                   | filter        | Controla paquetes **salientes** desde el host                                 | Normal                                                                  |
| **FORWARD**                  | filter        | Controla paquetes **que atraviesan** el host (para routing/NAT)               | Normal                                                                  |
| **DOCKER**                   | nat / filter  | Redirige tráfico externo a los contenedores según `-p`                        | Creada por Docker automáticamente                                       |
| **DOCKER-USER**              | filter        | Permite reglas **personalizadas** antes de que Docker aplique NAT/forwarding  | No se toca automáticamente, ideal para bloquear IPs                     |
| **DOCKER-FORWARD**           | filter        | Controla el **tráfico que pasa por contenedores** (forwarding interno Docker) | Se engancha en FORWARD, gestiona comunicación entre contenedores y host |
| **DOCKER-ISOLATION-STAGE-1** | filter        | Primer nivel de aislamiento de redes de contenedores                          | Separación básica entre redes de Docker                                 |
| **DOCKER-ISOLATION-STAGE-2** | filter        | Segundo nivel de aislamiento de redes de contenedores                         | Reglas más finas de separación, aplicado después de STAGE-1             |


| Opción | Qué hace                                           | Permite posición numérica |
| :----- | :------------------------------------------------- | :------------------------ |
| `-A`   | **Agrega al final** de la cadena                   | ❌ No                      |
| `-I`   | **Inserta al inicio o en una posición específica** | ✅ Sí                      |
```python
Entrada local al host:
   eth0 --> INPUT

Tráfico entre redes/contenedores:
   eth0 --> FORWARD --> DOCKER-USER --> DOCKER-FORWARD --> DOCKER

Salida desde el host:
   OUTPUT
```
## Comanos Iptables
```bash
# IPTABLES BASE (HOST)
sudo iptables -F                       # limpiar reglas
sudo iptables -P INPUT DROP            # politica por defecto
sudo iptables -P FORWARD DROP          # politica forward
sudo iptables -P OUTPUT ACCEPT         # salida permitida

# -------- LOOPBACK / ESTADO -----------
sudo iptables -A INPUT -i lo -j ACCEPT                        # loopback
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT  # conexiones activas

# -------- PUERTOS CLAVE -----------
sudo iptables -A INPUT -i eth0 -p tcp --dport 22 -j ACCEPT    # ssh
sudo iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT    # http
sudo iptables -A INPUT -i eth0 -p tcp --dport 443 -j ACCEPT   # https
sudo iptables -A INPUT -i eth0 -p udp --dport 53 -j ACCEPT    # dns udp
sudo iptables -A INPUT -i eth0 -p tcp --dport 53 -j ACCEPT    # dns tcp

# -------- BLOQUEO GLOBAL -----------
sudo iptables -A INPUT -i eth0 -j DROP                        # drop resto

# -------- DOCKER CONTROL -----------
sudo iptables -F DOCKER-USER                                  # limpiar cadena docker
sudo iptables -A DOCKER-USER -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT  # mantener sesiones
sudo iptables -A DOCKER-USER -i eth0 -p tcp --dport 80 -j ACCEPT    # permitir http contenedores
sudo iptables -A DOCKER-USER -i eth0 -p tcp --dport 443 -j ACCEPT   # permitir https contenedores
sudo iptables -A DOCKER-USER -i eth0 -j DROP                        # bloquear resto

# -------- GUARDAR REGLAS -----------
sudo apt install -y iptables-persistent     # persistencia
sudo netfilter-persistent save              # guardar reglas

# -------- VERIFICACION -----------
sudo iptables -L -n -v --line-numbers       # listar reglas
sudo iptables -L DOCKER-USER -n -v          # revisar docker
```