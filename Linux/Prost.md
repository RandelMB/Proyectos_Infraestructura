# ACTUALIZAR SISTEMA
```bash
sudo apt update && sudo apt upgrade -y                      # actualizar sistema completo
apt list --upgradable                                       # verificar paquetes pendientes
uname -a                                                    # verificar kernel
lsb_release -a                                              # verificar versión Ubuntu
```
# INSTALAR POSTGRESQL 16
```bash
sudo apt install -y postgresql postgresql-contrib           # instalar PostgreSQL
sudo systemctl enable postgresql                            # habilitar inicio automático
sudo systemctl start postgresql                             # iniciar servicio
sudo systemctl status postgresql --no-pager                 # verificar estado

psql --version                                              # verificar versión cliente
sudo -u postgres psql -c "SELECT version();"                # verificar versión servidor
```
# CREAR BASE DE DATOS Y USUARIO
```bash
sudo -u postgres psql <<'SQL'
CREATE DATABASE seguridad_db;                               -- crear base de datos
CREATE USER estudiante WITH ENCRYPTED PASSWORD '123456';   -- crear usuario
GRANT ALL PRIVILEGES ON DATABASE seguridad_db TO estudiante; -- asignar permisos
\q
SQL
```
# INSTALAR OPENSSL
```bash
sudo apt install -y openssl                                 # instalar openssl
openssl version                                             # verificar versión
which openssl                                               # verificar binario
```
# GENERAR CERTIFICADOS SSL/TLS
```bash
mkdir -p ~/postgresql_ssl && cd ~/postgresql_ssl            # crear directorio trabajo

openssl genrsa -out server.key 2048                         # generar clave privada RSA

openssl req -new -x509 \
-key server.key \
-out server.crt \
-days 365 \
-subj "/C=DO/ST=SantoDomingo/L=SantoDomingo/O=LAB/OU=DB/CN=postgres.local" # generar certificado

chmod 600 server.key                                        # permisos seguros clave
chmod 644 server.crt                                        # permisos certificado

ls -lh                                                      # verificar archivos
openssl x509 -in server.crt -text -noout | head             # verificar certificado
```
# COPIAR CERTIFICADOS A POSTGRESQL
```bash
sudo cp ~/postgresql_ssl/server.crt /var/lib/postgresql/    # copiar certificado
sudo cp ~/postgresql_ssl/server.key /var/lib/postgresql/    # copiar clave

sudo chown postgres:postgres /var/lib/postgresql/server.*   # asignar propietario
sudo chmod 600 /var/lib/postgresql/server.key               # asegurar clave privada
sudo chmod 644 /var/lib/postgresql/server.crt               # asegurar certificado

sudo ls -l /var/lib/postgresql/server.*                     # verificar permisos
```
# CONFIGURAR SSL EN POSTGRESQL
```bash
sudo sed -i "s/^#*ssl *=.*/ssl = on/" /etc/postgresql/18/main/postgresql.conf  # habilitar ssl

sudo tee -a /etc/postgresql/18/main/postgresql.conf >/dev/null <<'CONF'
ssl_cert_file = '/var/lib/postgresql/server.crt'            # ruta certificado
ssl_key_file = '/var/lib/postgresql/server.key'             # ruta clave privada
listen_addresses = '*'                                      # aceptar conexiones remotas
CONF

sudo grep -E "ssl|listen_addresses" /etc/postgresql/18/main/postgresql.conf # verificar configuración
```
# OBLIGAR CONEXIONES SSL
```bash
sudo cp /etc/postgresql/18/main/pg_hba.conf{,.bak}          # backup configuración

sudo sed -i '/^host\s\+all\s\+all\s\+0.0.0.0\/0\s\+md5/s/^/#/' \
/etc/postgresql/18/main/pg_hba.conf                         # deshabilitar conexiones inseguras

echo "hostssl all all 0.0.0.0/0 md5" | \
sudo tee -a /etc/postgresql/18/main/pg_hba.conf >/dev/null # permitir solo ssl

sudo tail -n 10 /etc/postgresql/18/main/pg_hba.conf         # verificar reglas
```
# REINICIAR Y VALIDAR SERVICIO
```bash
sudo systemctl restart postgresql                           # reiniciar servicio
sudo systemctl reload postgresql                            # recargar configuración
sudo systemctl status postgresql --no-pager                 # verificar estado

sudo ss -ltnp | grep 5432                                   # verificar puerto escuchando
journalctl -u postgresql -n 50 --no-pager                   # revisar logs
```
# VERIFICAR SSL DESDE POSTGRESQL
```bash
sudo -u postgres psql -c "SHOW ssl;"                        # verificar ssl habilitado

sudo -u postgres psql -c "
SELECT version();                                            -- verificar versión
"

sudo -u postgres psql -c "
SELECT * FROM pg_stat_ssl;                                   -- conexiones ssl activas
"
```
# PROBAR CONEXIÓN SSL EXITOSA
```bash
psql "host=127.0.0.1 dbname=seguridad_db user=estudiante sslmode=require" # conexión ssl requerida

psql "host=<IP_SERVIDOR> dbname=seguridad_db user=estudiante sslmode=require" # conexión remota ssl

PGSSLMODE=require \
psql -h <IP_SERVIDOR> -U estudiante -d seguridad_db         # variante usando variable entorno
```
# PROBAR BLOQUEO SIN SSL
```bash
psql "host=<IP_SERVIDOR> dbname=seguridad_db user=estudiante sslmode=disable" # conexión insegura

PGSSLMODE=disable \
psql -h <IP_SERVIDOR> -U estudiante -d seguridad_db         # variante insegura

# resultado esperado:
# FATAL: no pg_hba.conf entry for host ...
# SSL off
```
# VALIDAR CIFRADO ACTIVO
```bash
sudo tcpdump -i any port 5432 -nn                           # inspeccionar tráfico PostgreSQL

sudo apt install -y wireshark tshark                        # instalar herramientas análisis

sudo tshark -i any -f "tcp port 5432"                       # capturar tráfico cifrado

openssl s_client -connect <IP_SERVIDOR>:5432 -starttls postgres # validar handshake tls
```
# CONFIGURACIÓN FIREWALL
```bash
sudo ufw allow 5432/tcp                                     # permitir puerto PostgreSQL
sudo ufw enable                                             # habilitar firewall
sudo ufw status numbered                                    # verificar reglas

sudo iptables -L -n                                         # verificar reglas iptables
```
# BACKUP CONFIGURACIÓN
```bash
sudo cp /etc/postgresql/16/main/postgresql.conf ~/postgresql.conf.bak # backup config principal
sudo cp /etc/postgresql/16/main/pg_hba.conf ~/pg_hba.conf.bak         # backup control acceso

tar -czvf postgres_ssl_backup.tar.gz \
~/postgresql_ssl \
/etc/postgresql/16/main/postgresql.conf \
/etc/postgresql/16/main/pg_hba.conf                         # comprimir respaldo
```
# VERIFICACIONES FINALES
```bash
sudo -u postgres psql -c "SHOW ssl;"                        # confirmar ssl=on

sudo -u postgres psql -c "
SELECT datname, usename, client_addr, ssl
FROM pg_stat_ssl
JOIN pg_stat_activity
ON pg_stat_ssl.pid = pg_stat_activity.pid;
"                                                            # verificar clientes ssl

grep -n "hostssl" /etc/postgresql/16/main/pg_hba.conf       # validar regla hostssl

sudo systemctl is-enabled postgresql                         # verificar autostart
sudo systemctl is-active postgresql                          # verificar activo
```