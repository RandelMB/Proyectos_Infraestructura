![[9401a58c-1699-4dae-8475-bccd78942ea2.png]]
```sh
sudo apt update                                         # actualiza repositorios
sudo apt install -y git                                 # instala git

git --version                                           # verifica instalación

git config --global user.name "Tu Nombre"               # define tu nombre en git
git config --global user.email "tu@email.com"           # define tu correo en git
git config --list                                       # muestra configuración actual

git config --global credential.helper store             # guarda credenciales (opcional)

cd /ruta/de/tu/proyecto                                 # entra a la carpeta del proyecto
git init                                                # inicializa repositorio git local

git add .                                               # agrega todos los archivos al staging
git commit -m "primer commit"                           # crea el primer commit

git remote add origin https://github.com/usuario/repositorio.git   # conecta con GitHub
git remote set-url origin https://github.com/usuario/repositorio.git

git branch -M main                                      # renombra rama principal a main
git branch                 # muestra ramas locales
git branch -a              # muestra todas (locales + remotas)
git branch -r              # muestra solo ramas remotas
git show-branch            # muestra ramas con commits resumidos
git branch -vv             # muestra ramas con tracking (a qué remoto apuntan)

git branch -M main                                      # renombra rama principal a main
git push -u origin main                                 # sube el proyecto a GitHub

git clone https://github.com/usuario/repositorio.git    # clona un repositorio existente
git pull                                                # trae cambios del repositorio remoto
git push                                                # sube cambios al repositorio remoto



#------------------- AUTENTICARSE CON SSH-KEY CASO1------------------------------------
ls ~/.ssh                                                     # verifica si ya existe clave SSH

ssh-keygen -t ed25519 -C "tu@email.com"                      # crea nueva clave SSH

cat ~/.ssh/id_ed25519.pub                                    # muestra la clave pública (copiar a GitHub)

eval "$(ssh-agent -s)"                                       # inicia agente SSH
ssh-add ~/.ssh/id_ed25519                                    # agrega la clave al agente

ssh -T git@github.com                                        # prueba conexión con GitHub

git remote set-url origin git@github.com:RandelMB/zabbix-netbox-unified.git   # asegura remote SSH
git branch -M main                                           # asegura rama main
git push -u origin main                                      # sube el repositorio
```


```sh
#-------------- CASO2 GITHUB ----------------
ls ~/.ssh                                                     # verifica si ya existe clave SSH
ssh-keygen -t ed25519 -f ~/.ssh/id2_ed25519                   # crea nueva clave SSH\
cat ~/.ssh/id_ed25519.pub                                     # muestra la clave pública (copiar a GitHub)
 
echo "# tomcat" >> README.md                                  # Crea rename
git init                                                      # inicia repositorio local
git add README.md                                             #

git config --global user.name "Tu Nombre"                     # define tu nombre en git
git config --global user.email "tu@email.com"                 # define tu correo en git
git config --list                                             # muestra configuración actual

git commit -m "first commit"                                   #
git branch -M Rama                                             # renombra rama principal a main
git remote add origin git@github.com:TuOYo/tomcat.git          #
git push -u origin Rama                                        # sube el proyecto a GitHub

# ------------------ ACTUALIZAR RAMA --------------
git add .
git commit -m "actualización"
git push origin Rama
```

# NUEVA RAMA
```bash
# CREAR RAMA NUEVA (LOCAL)
git checkout -b nueva-ram              # crear y cambiar a la rama

# CREAR RAMA DESDE MAIN/MASTER
git checkout main                       # ir a rama base
git pull                                # actualizar
git checkout -b nueva-rama              # crear nueva rama

# SUBIR RAMA AL REMOTO
git push -u origin nueva-rama           # publicar rama y tracking

# CAMBIAR ENTRE RAMAS
git checkout otra-rama                  # cambiar de rama

# VERIFICACION
git branch                              # listar ramas locales
git branch -r                           # listar remotas
git status                              # confirmar rama actual
```

# SSH KEY (REEMPLAZO COMPLETO GITHUB
```bash
# GENERACIÓN
ssh-keygen -t ed25519 -C "tu_email@example.com" -f ~/.ssh/id_ed25519_new   # crear nueva llave SSH

# AGENTE SSH
eval "$(ssh-agent -s)"                                                     # iniciar agente SSH
ssh-add ~/.ssh/id_ed25519_new                                              # cargar nueva llave

# EXPORTAR CLAVE
cat ~/.ssh/id_ed25519_new.pub                                              # mostrar clave pública (copiar a GitHub)

# CONFIGURACIÓN SSH
mkdir -p ~/.ssh                                                            # asegurar directorio .ssh
chmod 700 ~/.ssh                                                           # permisos correctos
printf "Host github.com\n  HostName github.com\n  User git\n  IdentityFile ~/.ssh/id_ed25519_new\n" >> ~/.ssh/config  # forzar uso de llave
chmod 600 ~/.ssh/config                                                    # permisos config

# LIMPIEZA (OPCIONAL)
ssh-add -D                                                                 # eliminar llaves cargadas previamente
ssh-add ~/.ssh/id_ed25519_new                                              # recargar solo la nueva

# VERIFICACIÓN
ssh -T git@github.com                                                      # probar autenticación SSH
git remote -v                                                              # verificar repositorio remoto
git push origin main                                                       # probar push
```

# INICIALIZAR ESTRUCTURA BASE
```bash
git checkout main                              # ir a rama principal
git pull origin main                           # actualizar estado
git tag v1.0                                   # marcar versión estable inicial
git push origin main --tags                    # subir cambios + tags
```
# CREAR RAMA DE DESARROLLO
```bash
git checkout -b feature/nuevo-sistema          # nueva funcionalidad aislada
git push -u origin feature/nuevo-sistema       # publicar rama
```
# CREAR RESPALDO (LEGACY)
```bash
git checkout -b legacy/sistema-anterior        # rama para versión vieja
git push -u origin legacy/sistema-anterior     # subir respaldo
```
# TRABAJO DIARIO SEGURO
```bash
git add .                                      # agregar cambios
git commit -m "feat: nueva lógica pagos"       # commit estructurado
git push                                       # subir cambios
```
# INTEGRAR CAMBIOS CONTROLADOS
```bash
git checkout main                              # volver a main
git pull origin main                           # actualizar
git merge feature/nuevo-sistema                # integrar cambios
git push origin main                           # subir merge
```
# VERSIONAR NUEVA RELEASE
```bash
git tag v2.0                                   # marcar nueva versión
git push origin v2.0                           # publicar tag
```
# ACTUALIZAR CAMBIOS
```bash
git add .                                               # agrega todos los archivos al staging
git commit -m "primer commit"                           # crea el primer commit
git push origin -u rama-actualizada                     # subir rama
```
# RENOMBRAR RAMA LOCAL
```bash
git branch -m nueva-rama                 # renombra rama actual
git branch -m vieja-rama nueva-rama      # renombra rama específica
```
# ACTUALIZAR REMOTO
```
git push origin -u nueva-rama            # subir nueva rama
git push origin --delete vieja-rama      # eliminar rama antigua remota
```
# SINCRONIZAR REFERENCIAS
```bash
git fetch --prune                        # limpiar referencias obsoletas
```
# VERIFICACIÓN
```bash
git branch -a                            # listar ramas
git status                               # confirmar rama activa
```