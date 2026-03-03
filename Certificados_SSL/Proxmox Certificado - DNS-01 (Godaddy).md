
# DNS-01 Proxmox

```bash
### 1. Crear rutas necesarias
mkdir -p /etc/pve/priv/acme/dnsapi
mkdir -p /etc/pve/acme

# Descargar el plugin de GoDaddy
curl -o /etc/pve/priv/acme/dnsapi/dns_gd.sh https://raw.githubusercontent.com/acmesh-official/acme.sh/master/dnsapi/dns_gd.sh

# Crear archivo con credenciales GoDaddy
nano /etc/pve/priv/acme/godaddy.conf

# Contenido:
GODADDY_API_KEY="tu_api_key"
GODADDY_API_SECRET="tu_api_secret"

# Instalar acme.sh
apt update
apt install socat curl -y
curl https://get.acme.sh | sh

# Registrar la cuenta en Let’s Encrypt
~/.acme.sh/acme.sh --register-account -m Carlos@ejemplo.net --server letsencrypt

# Comprobar
~/.acme.sh/acme.sh --info

# 6. Solicitar certificado wildcard usando DNS-01
~/.acme.sh/acme.sh --issue --dns dns_gd -d cybersupports.net -d '*.cybersupports.net' --server letsencrypt

#Instalar certificado en Proxmox
cp ~/.acme.sh/cybersupports.net_ecc/fullchain.cer /etc/pve/nodes/proxmox/pve-ssl.pem
cp ~/.acme.sh/cybersupports.net_ecc/cybersupports.net.key /etc/pve/nodes/proxmox/pve-ssl.key
systemctl restart pveproxy

#Este comando es para la Renovación de certificado
~/.acme.sh/acme.sh --renew -d cybersupports.net --dns dns_gd --server letsencrypt

# en caso de que tenga que forzar la renovación:
~/.acme.sh/acme.sh --renew -d cybersupports.net --dns dns_gd --force --server letsencrypt
```

![[Pasted image 20260228191235.png]]