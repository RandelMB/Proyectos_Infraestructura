# SSH

## CONFIGURACION SEGURA SSH
```bash
sudo nano /etc/ssh/sshd_config                              # editar configuración SSH

PermitRootLogin no                                          # deshabilitar root login
PasswordAuthentication no                                   # deshabilitar password auth
PubkeyAuthentication yes                                    # habilitar llaves públicas
Port 2222                                                   # cambiar puerto SSH
MaxAuthTries 3                                              # limitar intentos
LoginGraceTime 30                                           # timeout autenticación
AllowUsers usuario1 usuario2                                # usuarios permitidos

sudo sshd -t                                                # validar configuración
sudo systemctl restart ssh                                  # reiniciar SSH
sudo systemctl reload ssh                                   # recargar configuración
sudo systemctl status ssh                                   # estado servicio
```
## VERIFICACION SSH
```bash
ss -tulnp | grep ssh                                        # puerto escuchando
netstat -tulnp | grep ssh                                   # alternativa netstat

journalctl -u ssh -n 50                                     # últimos logs
journalctl -fu ssh                                          # logs tiempo real

whoami                                                      # usuario actual
id                                                          # UID/GID actual
groups                                                      # grupos actuales

env | grep SSH                                              # variables SSH
history | grep ssh                                          # historial SSH
```
## GENERAR LLAVES SSH
```bash
ssh-keygen -t rsa -b 4096 -C "ejemp@ejemplo.com"            # RSA 4096
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/id_ecdsa_521           # ECDSA 521
ssh-keygen -t ed25519                                       # Ed25519 rápida
ssh-keygen -t ed25519 -f ~/.ssh/id2_ed25519                 # Ed25519 personalizada

ssh-keygen -y -f ~/.ssh/id_ed25519                          # pública desde privada
ssh-keygen -p -f ~/.ssh/id_ed25519                          # pedir passphrase actual

# Opciones comunes
-t ed25519                                                  # algoritmo
-b 4096                                                     # tamaño llave
-f ~/.ssh/key                                               # nombre archivo
-N "clave"                                                  # passphrase
-C "comentario"                                             # comentario
```
## CLIENTE SSH Y DEBUG
```bash
ssh -i key1 usuario1@ip                                     # acceso válido
ssh -i key2 usuario2@ip                                     # acceso válido

ssh -i key2 usuario1@ip                                     # prueba inválida
ssh -i key1 usuario2@ip                                     # prueba inválida

ssh -vvv -i key usuario@ip                                  # debug detallado
ssh -p 2222 usuario@ip                                      # puerto custom
ssh -o StrictHostKeyChecking=no usuario@ip                  # ignorar fingerprint
```

## ARCHIVOS Y PERMISOS SSH

```bash
mkdir -p ~/.ssh                                             # crear carpeta SSH
chmod 700 ~/.ssh                                            # permisos carpeta

nano ~/.ssh/authorized_keys                                 # agregar claves
chmod 600 ~/.ssh/authorized_keys                            # permisos archivo

chown -R $USER:$USER ~/.ssh                                 # ownership correcto
namei -l ~/.ssh/authorized_keys                             # validar permisos completos

ls -lah ~/.ssh                                              # listar archivos SSH
```

## VALIDAR SSHD_CONFIG

```bash
sshd -t                                                     # validar sintaxis

grep -E "^(Port|PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|AllowUsers)" /etc/ssh/sshd_config   # validar parámetros

ls -ld /home2/keys                                          # permisos directorio llaves
ls -l /home2/keys/$(whoami)                                 # permisos llaves usuario
```
## WRAPPER DOCKER SAFE
```bash
ls -l /usr/local/bin/docker-safe                            # permisos wrapper
file /usr/local/bin/docker-safe                             # tipo archivo
head -n 20 /usr/local/bin/docker-safe                       # revisar contenido
```
## VALIDAR SUDOERS
```bash
visudo -c                                                   # validar sintaxis sudoers
sudo -l -U agent1                                           # permisos usuario
```
## TEST DOCKER CONTROLADO
```bash
sudo -u agent1 sudo docker-safe ps                          # listar contenedores
sudo -u agent1 sudo docker-safe run hello-world             # prueba contenedor
```
## RESTRICCIONES DOCKER
```bash
gpasswd -d agent1 docker                                    # quitar grupo docker
ls -l /var/run/docker.sock                                  # permisos socket Docker
```
## VALIDACIONES SEGURIDAD
```bash
sudo docker-safe run -v /opt/stacks/agent1:/app nginx       # permitido
sudo docker-safe run -v /:/host ubuntu                      # debe fallar
sudo docker-safe run --privileged ubuntu                    # debe fallar
```

# USUARIOS

## AUTENTICACION Y CONTEXTO
```bash
sudo -i                                                     # shell root login
sudo -s                                                     # shell root simple
sudo -H -i                                                  # root HOME limpio
sudo -u root -i                                             # root explícito

su                                                          # cambiar a root
su -                                                        # root limpio
su usuario                                                  # cambiar usuario
su - usuario                                                # entorno limpio usuario

runuser -l usuario                                          # cambiar usuario sin password

whoami                                                      # usuario actual
logname                                                     # usuario login real

id                                                          # UID/GID actual
id usuario                                                  # UID/GID usuario

sudo -u usuario comando                                     # ejecutar como usuario
sudo -u usuario -H comando                                  # HOME usuario

sudo -E comando                                             # preservar entorno
sudo env                                                    # variables entorno sudo

sudo -k                                                     # invalidar credenciales
sudo -K                                                     # limpiar cache sudo
sudo -v                                                     # refrescar sudo
```

## SUDO Y PRIVILEGIOS

```bash
visudo                                                      # editar sudoers
visudo -c                                                   # validar sintaxis
visudo -f /etc/sudoers.d/custom                             # editar include

sudo -l                                                     # permisos actuales
sudo -U usuario -l                                          # permisos usuario

sudo usermod -aG sudo usuario                               # agregar sudo
usermod -aG sudo usuario                                    # agregar grupo sudo

gpasswd -a usuario sudo                                     # agregar via gpasswd
deluser usuario sudo                                        # quitar sudo
gpasswd -d usuario sudo                                     # quitar grupo sudo

usermod -G sudo usuario                                     # solo grupo sudo
usermod -aG wheel usuario                                   # grupo wheel

grep sudo /etc/group                                        # miembros sudo
```

## PASSWORD Y BLOQUEO

```bash
passwd                                                      # cambiar password
passwd usuario                                              # cambiar password usuario

sudo passwd root                                            # password root

passwd -l usuario                                           # bloquear usuario
passwd -u usuario                                           # desbloquear usuario
passwd -S usuario                                           # estado password

passwd -d usuario                                           # eliminar password

chage -l usuario                                            # expiración cuenta
chage -E 0 usuario                                          # expirar cuenta
chage -E -1 usuario                                         # sin expiración

chage -M 90 usuario                                         # máximo días
chage -m 7 usuario                                          # mínimo días
chage -W 7 usuario                                          # aviso expiración

chage -d 0 usuario                                          # forzar cambio login

usermod -L usuario                                          # lock alterno
usermod -U usuario                                          # unlock alterno
```

## CREAR USUARIO

```bash
adduser usuario                                             # crear interactivo
adduser --disabled-password usuario                         # sin password
adduser --system usuario                                    # usuario sistema

adduser --home /custom usuario                              # home custom
adduser --shell /bin/bash usuario                           # shell custom

useradd usuario                                             # básico
useradd -m usuario                                          # crear home
useradd -M usuario                                          # sin home

useradd -d /home/custom usuario                             # home específico
useradd -s /bin/bash usuario                                # shell específica

useradd -u 1001 usuario                                     # UID específico
useradd -g grupo usuario                                    # grupo principal
useradd -G sudo usuario                                     # grupo secundario

useradd -r usuario                                          # usuario sistema
useradd -c "desc" usuario                                   # comentario

useradd -k /etc/skel usuario                                # copiar skel
```

## ELIMINAR USUARIO

```bash
deluser usuario                                             # eliminar usuario
deluser --remove-home usuario                               # eliminar home
deluser --backup usuario                                    # backup home
deluser --remove-all-files usuario                          # eliminar archivos

userdel usuario                                             # bajo nivel
userdel -r usuario                                          # eliminar home+mail
userdel -f usuario                                          # forzar eliminación
userdel -Z usuario                                          # cleanup SELinux
```

## MODIFICAR USUARIO

```bash
usermod -l nuevo viejo                                      # cambiar nombre
usermod -d /home/nuevo -m usuario                           # mover home

usermod -s /bin/bash usuario                                # cambiar shell
usermod -u 2000 usuario                                     # cambiar UID

usermod -g grupo usuario                                    # grupo principal
usermod -aG grupo usuario                                   # agregar grupo
usermod -G g1,g2 usuario                                    # setear grupos

usermod -c "desc" usuario                                   # comentario

usermod -L usuario                                          # bloquear cuenta
usermod -U usuario                                          # desbloquear cuenta

usermod -e YYYY-MM-DD usuario                               # expiración
usermod -f 30 usuario                                       # inactividad
usermod --expiredate 2026-12-31 usuario                     # expiración larga
```

## GESTION DE GRUPOS

```bash
groupadd grupo                                              # crear grupo
groupadd -g 2000 grupo                                      # GID específico
groupadd -r grupo                                           # grupo sistema

groupdel grupo                                              # eliminar grupo

groupmod -n nuevo viejo                                     # renombrar grupo
groupmod -g 3000 grupo                                      # cambiar GID
groupmod -o -g 3000 grupo                                   # GID no único
```

## MEMBRESIA DE GRUPOS

```bash
gpasswd -a usuario grupo                                    # agregar miembro
gpasswd -d usuario grupo                                    # quitar miembro

gpasswd -M u1,u2 grupo                                      # definir miembros
gpasswd -A admin grupo                                      # admin grupo

newgrp grupo                                                # cambiar grupo activo
sg grupo -c "comando"                                       # ejecutar comando grupo

usermod -aG grupo usuario                                   # agregar via usermod
deluser usuario grupo                                       # quitar grupo
```

## CONSULTA DE USUARIOS

```bash
getent passwd                                               # listar usuarios
getent passwd usuario                                       # info usuario

cat /etc/passwd                                             # usuarios locales
less /etc/passwd                                            # paginado

cut -d: -f1 /etc/passwd                                     # nombres usuarios
awk -F: '{print $1}' /etc/passwd                            # nombres awk

grep usuario /etc/passwd                                    # buscar usuario

grep -E "nologin|false" /etc/passwd                         # sin shell
grep -vE "nologin|false" /etc/passwd                        # con shell

wc -l /etc/passwd                                           # total usuarios
compgen -u                                                  # usuarios bash
```

## CONSULTA DE GRUPOS

```bash
getent group                                                # listar grupos
getent group grupo                                          # info grupo

cat /etc/group                                              # grupos locales
less /etc/group                                             # ver paginado

cut -d: -f1 /etc/group                                      # nombres grupos
awk -F: '{print $1}' /etc/group                             # nombres awk

grep usuario /etc/group                                     # grupos usuario
wc -l /etc/group                                            # total grupos
```

## CONSULTA MEMBRESIA

```bash
groups                                                      # grupos actuales
groups usuario                                              # grupos usuario

id -nG usuario                                              # grupos nombre
id -Gn usuario                                              # formato alterno
id -gn usuario                                              # grupo principal

getent group | grep usuario                                 # grupos via DB
grep usuario /etc/group                                     # grupos archivo
```

## SESIONES Y ACTIVIDAD

```bash
who                                                         # usuarios conectados
w                                                           # usuarios + procesos
users                                                       # lista simple

last                                                        # historial logins
last usuario                                                # historial usuario

lastlog                                                     # último login
lastlog -u usuario                                          # último login usuario

uptime                                                      # tiempo activo
```

## ARCHIVOS DEL SISTEMA

```bash
getent shadow                                               # shadow DB
sudo cat /etc/shadow                                        # ver shadow

pwck                                                        # validar passwd
grpck                                                       # validar grupos

pwconv                                                      # passwd -> shadow
pwunconv                                                    # revertir shadow

grpconv                                                     # group -> gshadow
grpunconv                                                   # revertir gshadow
```

## VERIFICACION USUARIOS

```bash
id usuario                                                  # verificar UID/GID
groups usuario                                              # verificar grupos

sudo -U usuario -l                                          # verificar sudo

getent passwd usuario                                       # verificar usuario
getent group grupo                                          # verificar grupo

passwd -S usuario                                           # estado cuenta
chage -l usuario                                            # expiración cuenta
```

# USUARIOS PARA DOCKER

## CREAR USUARIO PARA DOCKER

```bash
useradd -m -s /bin/bash usuario                             # crear usuario
passwd usuario                                              # asignar password

usermod -aG docker usuario                                  # acceso Docker

id usuario                                                  # verificar grupos
```

## MAPEAR UID/GID HOST

```bash
id -u usuario                                               # obtener UID
id -g usuario                                               # obtener GID

docker run -u $(id -u usuario):$(id -g usuario) -it imagen bash   # ejecutar con UID/GID

docker exec -u $(id -u usuario):$(id -g usuario) -it contenedor bash   # entrar contenedor
```

## EXPORTAR UID/GID PARA COMPOSE

```bash
id -u usuario                                               # obtener UID
id -g usuario                                               # obtener GID

export UID=$(id -u usuario)                                 # exportar UID
export GID=$(id -g usuario)                                 # exportar GID
```

## DOCKER-COMPOSE CON UID/GID

```bash
cat <<EOF > docker-compose.yml
version: "3.9"

services:
  app:
    image: ubuntu:22.04
    container_name: app

    user: "${UID}:${GID}"

    volumes:
      - /home/usuario:/data

    tty: true
EOF

docker compose up -d                                        # iniciar stack
```

## CREAR USUARIO EN CONTENEDOR

```bash
docker exec -it contenedor useradd -m -s /bin/bash usuario  # crear usuario
docker exec -it contenedor passwd usuario                   # asignar password

docker exec -it contenedor usermod -aG sudo usuario         # agregar sudo
docker exec -it contenedor id usuario                       # verificar usuario
```

## DOCKERFILE CON USUARIO

```bash
echo -e "FROM ubuntu:22.04\nRUN useradd -m -s /bin/bash usuario\nUSER usuario\nWORKDIR /home/usuario" > Dockerfile   # crear Dockerfile

docker build -t imagen_usuario .                            # construir imagen
docker run -it imagen_usuario bash                          # ejecutar imagen
```

## VERIFICACION DOCKER USER

```bash
docker exec -it contenedor whoami                           # usuario actual
docker exec -it contenedor id                               # UID/GID activo

docker inspect contenedor | grep -i user                    # verificar config usuario
```