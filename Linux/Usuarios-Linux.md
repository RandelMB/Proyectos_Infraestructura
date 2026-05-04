


# SSH
```sh
# ----------CONFIGURAR SSH SEGURO------------

sudo nano /etc/ssh/sshd_config                 # editar configuracion principal de SSH  
  
PermitRootLogin no                            # deshabilitar login directo como root  
PasswordAuthentication no                     # deshabilitar autenticacion por contraseña  
PubkeyAuthentication yes                      # habilitar autenticacion por llave publica  
Port 2222                                     # cambiar puerto por defecto (22)  
MaxAuthTries 3                                # limitar intentos de autenticacion  
LoginGraceTime 30                             # tiempo maximo para autenticarse  
AllowUsers usuario1 usuario2                  # restringir acceso a usuarios especificos  
  
sudo sshd -t                                  # validar configuracion antes de aplicar  
sudo systemctl restart ssh                    # reiniciar servicio SSH  
sudo systemctl status ssh                     # verificar estado del servicio


# ----------VERIFICACION DE ACCESO SSH------------
ss -tulnp | grep ssh                         # verificar puerto escuchando  
journalctl -u ssh -n 50                      # revisar ultimos logs del servicio
whoami                                           # usuario actual
id                                               # UID/GID y grupos
groups                                           # grupos del usuario
env | grep SSH                                   # variables SSH activas
history | grep ssh                               # historial relacionado a SSH
```

## Generar la clave SSH desde Window
```bash
# --------------------GENERADOR DE LLAVES--------------------------
ssh-keygen -t rsa -b 4096 -C "ejemp@ejemplo.com"        # clave RSA fuerte
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/id_ecdsa_521       # ECDSA 521 bits
ssh-keygen -t ed25519 -f ~/.ssh/id2_ed25519             # Ed25519 (recomendada)
ssh-keygen -t ed25519                                   # rápida
ssh-keygen -y -f ~/.ssh/id_ed25519                      # obtener pública desde privada

# Opciones
-t ed25519                                              # algoritmo
-b 4096                                                 # tamaño
-f ~/.ssh/key                                           # nombre archivo
-N "clave"                                              # passphrase
-C "comentario"                                         # etiqueta

# --------------------SSH CLIENTE (PRUEBAS)--------------------------
ssh -i key1 usuario1@ip                                 # acceso correcto
ssh -i key2 usuario2@ip                                 # acceso correcto

ssh -i key2 usuario1@ip                                 # debe fallar
ssh -i key1 usuario2@ip                                 # debe fallar

ssh -vvv -i key usuario@ip                              # debug detallado

# --------------------SSH (ARCHIVOS Y PERMISOS)--------------------------
mkdir -p ~/.ssh                                         # crear carpeta
chmod 700 ~/.ssh                                        # permiso correcto
nano ~/.ssh/authorized_keys                             # agregar claves públicas
chmod 600 ~/.ssh/authorized_keys                        # proteger archivo
chown -R $USER:$USER ~/.ssh                             # propiedad correcta

# validar permisos completos
namei -l ~/.ssh/authorized_keys




# ---VALIDAR SSHD CONFIG--- #
sshd -t                                          # validar sintaxis sshd_config
grep -E "^(Port|PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|AllowUsers)" /etc/ssh/sshd_config   # validar parametros clave
ls -ld /home2/keys                               # verificar permisos del directorio de llaves
ls -l /home2/keys/$(whoami)                      # verificar llave del usuario actual



# ---VALIDAR WRAPPER DOCKER--- #
ls -l /usr/local/bin/docker-safe                 # permisos del wrapper
file /usr/local/bin/docker-safe                  # tipo de archivo
head -n 20 /usr/local/bin/docker-safe            # revisar contenido inicial

# ---VALIDAR SUDOERS--- #
visudo -c                                        # validar sintaxis global
sudo -l -U agent1                                # permisos asignados a agent1

# ---TEST EJECUCION CONTROLADA--- #
sudo -u agent1 sudo docker-safe ps               # probar ejecucion docker restringida
sudo -u agent1 sudo docker-safe run hello-world  # prueba de contenedor controlado


# --------------------RESTRICCIONES DOCKER IMPORTANTES--------------------------
gpasswd -d agent1 docker                               # quitar acceso directo docker
ls -l /var/run/docker.sock                             # verificar acceso socket

# --------------------VALIDACIONES CLAVE--------------------------
# probar acceso docker controlado
sudo docker-safe run -v /opt/stacks/agent1:/app nginx   # permitido
sudo docker-safe run -v /:/host ubuntu                  # debe FALLAR
sudo docker-safe run --privileged ubuntu                # debe FALLAR
```

# Usuarios
# AUTENTICACION / CONTEXTO

```
sudo -i                              # shell root login
sudo -s                              # shell root sin login
sudo -H -i                           # root con HOME limpio
sudo -u root -i                      # root explícito
su                                   # cambiar a root
su -                                 # root con entorno limpio
su usuario                           # cambiar a usuario
su - usuario                         # entorno limpio usuario
runuser -l usuario                   # cambiar usuario sin password root
whoami                               # usuario actual
logname                              # usuario login real
id                                   # uid/gid actual
id usuario                           # uid/gid de usuario
sudo -u usuario comando              # ejecutar como otro usuario
sudo -u usuario -H comando           # con HOME del usuario
sudo -E comando                      # preservar entorno
sudo env                             # ver entorno sudo
sudo -k                              # invalidar credenciales
sudo -K                              # limpiar cache sudo
sudo -v                              # refrescar credenciales
```

# SUDO / PRIVILEGIOS
```
visudo                               # editar sudoers seguro
visudo -c                            # validar sintaxis
visudo -f /etc/sudoers.d/custom      # editar include
sudo -l                              # permisos actuales
sudo -U usuario -l                   # permisos otro usuario
sudo usermod -aG sudo usuario        # agregar a sudo
usermod -aG sudo usuario             # agregar sin sudo
gpasswd -a usuario sudo              # agregar via gpasswd
deluser usuario sudo                 # quitar de sudo
gpasswd -d usuario sudo              # quitar via gpasswd
usermod -G sudo usuario              # setear solo sudo
usermod -aG wheel usuario            # grupo wheel
grep sudo /etc/group                 # ver miembros sudo
```

# PASSWORD / BLOQUEO
```
passwd                               # cambiar password actual
passwd usuario                       # cambiar password usuario
sudo passwd root                     # cambiar password root
passwd -l usuario                    # bloquear cuenta
passwd -u usuario                    # desbloquear cuenta
passwd -S usuario                    # estado password
passwd -d usuario                    # eliminar password
chage -l usuario                     # info expiración
chage -E 0 usuario                   # expirar cuenta
chage -E -1 usuario                  # sin expiración
chage -M 90 usuario                  # max días
chage -m 7 usuario                   # min días
chage -W 7 usuario                   # aviso expiración
chage -d 0 usuario                   # forzar cambio login
usermod -L usuario                   # lock alterno
usermod -U usuario                   # unlock alterno
```

# CREAR USUARIO
```
adduser usuario                      # crear interactivo
adduser --disabled-password usuario  # sin password
adduser --system usuario             # usuario sistema
adduser --home /custom usuario       # home custom
adduser --shell /bin/bash usuario    # shell específica
useradd usuario                      # básico
useradd -m usuario                   # con home
useradd -M usuario                   # sin home
useradd -d /home/custom usuario      # definir home
useradd -s /bin/bash usuario         # definir shell
useradd -u 1001 usuario              # UID específico
useradd -g grupo usuario             # grupo principal
useradd -G sudo usuario              # grupos secundarios
useradd -r usuario                   # usuario sistema
useradd -c "desc" usuario            # comentario
useradd -k /etc/skel usuario         # copiar skel
```

# ELIMINAR USUARIO
```
deluser usuario                      # eliminar usuario
deluser --remove-home usuario        # eliminar + home
deluser --backup usuario             # backup home
deluser --remove-all-files usuario   # eliminar archivos
userdel usuario                      # bajo nivel
userdel -r usuario                   # eliminar + home + mail
userdel -f usuario                   # forzar eliminación
userdel -Z usuario                   # SELinux cleanup
```
# MODIFICAR USUARIO
```
usermod -l nuevo viejo               # cambiar nombre
usermod -d /home/nuevo -m usuario    # mover home
usermod -s /bin/bash usuario         # cambiar shell
usermod -u 2000 usuario              # cambiar UID
usermod -g grupo usuario             # grupo principal
usermod -aG grupo usuario            # agregar grupo
usermod -G g1,g2 usuario             # setear grupos
usermod -c "desc" usuario            # comentario
usermod -L usuario                   # bloquear cuenta
usermod -U usuario                   # desbloquear cuenta
usermod -e YYYY-MM-DD usuario        # expiración
usermod -f 30 usuario                # inactividad
usermod --expiredate 2026-12-31 u    # expiración larga
```
# GRUPOS (GESTION)
```
groupadd grupo                       # crear grupo
groupadd -g 2000 grupo               # GID específico
groupadd -r grupo                    # grupo sistema
groupdel grupo                       # eliminar grupo
groupmod -n nuevo viejo              # renombrar
groupmod -g 3000 grupo               # cambiar GID
groupmod -o -g 3000 grupo            # GID no único
```
# GRUPOS (MIEMBROS)
```
gpasswd -a usuario grupo             # agregar miembro
gpasswd -d usuario grupo             # eliminar miembro
gpasswd -M u1,u2 grupo               # setear miembros
gpasswd -A admin grupo               # admin grupo
newgrp grupo                         # cambiar grupo activo
sg grupo -c "comando"                # ejecutar con grupo
usermod -aG grupo usuario            # agregar via usermod
deluser usuario grupo                # quitar usuario grupo
```
# CONSULTA USUARIOS
```
getent passwd                        # listar usuarios
getent passwd usuario                # info usuario
cat /etc/passwd                      # usuarios locales
less /etc/passwd                     # ver paginado
cut -d: -f1 /etc/passwd              # nombres
awk -F: '{print $1}' /etc/passwd     # nombres awk
grep usuario /etc/passwd             # buscar usuario
grep -E "nologin|false" /etc/passwd  # sin shell
grep -vE "nologin|false" /etc/passwd # con shell
wc -l /etc/passwd                    # total usuarios
compgen -u                           # usuarios bash
```
# CONSULTA GRUPOS
```
getent group                         # listar grupos
getent group grupo                   # info grupo
cat /etc/group                       # grupos locales
less /etc/group                      # ver paginado
cut -d: -f1 /etc/group               # nombres
awk -F: '{print $1}' /etc/group      # nombres awk
grep usuario /etc/group              # grupos de usuario
wc -l /etc/group                     # total grupos
```
# CONSULTA MEMBRESIA
```
groups                               # grupos actuales
groups usuario                       # grupos usuario
id -nG usuario                       # nombres grupos
id -Gn usuario                       # formato alterno
id -gn usuario                       # grupo principal
getent group | grep usuario          # grupos via db
grep usuario /etc/group              # grupos archivo
```
# SESIONES / ACTIVIDAD
```
who                                  # usuarios conectados
w                                    # usuarios + procesos
users                                # lista simple
last                                 # historial logins
last usuario                         # historial usuario
lastlog                              # último login
lastlog -u usuario                   # último login usuario
uptime                               # tiempo activo
```
# ARCHIVOS / BASE DATOS SISTEMA
```
getent shadow                        # shadow (root)
sudo cat /etc/shadow                 # ver shadow
pwck                                 # verificar passwd
grpck                                # verificar group
pwconv                               # passwd -> shadow
pwunconv                             # revertir shadow
grpconv                              # group -> gshadow
grpunconv                            # revertir gshadow
```
# VERIFICACION
```
id usuario                           # verificar UID/GID
groups usuario                       # verificar grupos
sudo -U usuario -l                   # verificar sudo
getent passwd usuario                # verificar existencia
getent group grupo                   # verificar grupo
passwd -S usuario                    # estado cuenta
chage -l usuario                     # expiración
```

# USUARIOS PARA DOCKER
## CREAR USUARIO EN HOST (PARA DOCKER)
```bash
useradd -m -s /bin/bash usuario              # crear usuario con home
passwd usuario                               # asignar contraseña
usermod -aG docker usuario                   # acceso a docker sin sudo
id usuario                                   # verificar grupos
```
## MAPEAR USUARIO A CONTENEDOR (UID/GID)
```bash
id -u usuario                                # obtener UID
id -g usuario                                # obtener GID
docker run -u $(id -u usuario):$(id -g usuario) -it imagen bash   # ejecutar con UID/GID
docker exec -u $(id -u usuario):$(id -g usuario) -it contenedor bash # entrar como usuario host
```
## MAPEAR USUARIO DESDE HOST UID/GID
```sh
id -u usuario                                      # obtener UID
id -g usuario                                      # obtener GID
export UID=$(id -u usuario)                        # exportar UID
export GID=$(id -g usuario)                        # exportar GID
cat <<EOF > docker-compose.yml                     # crear compose con usuario
version: "3.9"
services:
  app:
    image: ubuntu:22.04
    container_name: app
    user: "${UID}:${GID}"                          # ejecutar con UID/GID host
    volumes:
      - /home/usuario:/data                        # permisos consistentes
    tty: true
EOF
docker compose up -d                               # levantar contenedor
```
## CREAR USUARIO DENTRO DEL CONTENEDOR
```bash
docker exec -it contenedor useradd -m -s /bin/bash usuario        # crear usuario dentro
docker exec -it contenedor passwd usuario                         # asignar contraseña
docker exec -it contenedor usermod -aG sudo usuario               # agregar a sudo (si aplica)
docker exec -it contenedor id usuario                             # verificar
```
## DOCKERFILE (USUARIO PERSONALIZADO)
```bash
echo -e "FROM ubuntu:22.04\nRUN useradd -m -s /bin/bash usuario\nUSER usuario\nWORKDIR /home/usuario" > Dockerfile   # crear Dockerfile
docker build -t imagen_usuario .             # construir imagen
docker run -it imagen_usuario bash           # ejecutar como usuario
```
## VERIFICACION
```bash
docker exec -it contenedor whoami            # usuario actual
docker exec -it contenedor id                # UID/GID activo
docker inspect contenedor | grep -i user     # verificar config usuario
```
