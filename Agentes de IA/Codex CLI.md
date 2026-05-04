#  WINDOWS 
```bash
# ----------INSTALAR NODE------------
winget install OpenJS.NodeJS.LTS             # instalar node LTS

# ----------VERIFICAR------------
node --version                               # version node
npm --version                                # version npm

# ----------INSTALAR CODEX------------
npm install -g @openai/codex                 # instalar CLI

# ----------VERIFICAR------------
codex --version                              # verificar instalacion

# ----------LOGIN------------
codex login                                  # autenticar

# ----------USO------------
codex                                        # ejecutar
```
# CODEX CLI  LINUX
```bash
# ----------INSTALACION------------
sudo apt update                          # actualiza repositorios
sudo apt install -y nodejs npm          # instala nodejs y npm
npm install -g @openai/codex            # instala codex CLI global

# ----------VERIFICACION------------
node -v                                 # version node
npm -v                                  # version npm
codex --version                         # version codex

# ----------LOGIN------------
codex login                             # login con cuenta
codex login status                      # estado autenticacion
export OPENAI_API_KEY="tu_api_key"      # api key manual (opcional)

# ----------BASICO------------
codex                                   # modo interactivo
codex "explica este proyecto"           # prompt directo
codex -C /ruta/proyecto                 # ejecutar en otro directorio

# ----------MODOS------------
codex --suggest                         # modo seguro (default)
codex --auto-edit                       # edicion automatica
codex --full-auto                       # ejecucion completa

# ----------MODELO------------
codex -m o4-mini                        # modelo rapido
codex --model gpt-5.3-codex             # modelo especifico

# ----------EJECUCION DIRECTA------------
codex exec "crear API en node"          # tarea puntual
codex exec "fix bug en main.py"         # correccion directa

# ----------REVISION------------
codex review --uncommitted              # revisa cambios locales
codex review --base main                # compara con rama

# ----------SESIONES------------
codex resume                            # listar sesiones
codex resume --last                     # reanudar ultima
codex fork                              # clonar sesion

# ----------IMAGENES------------
codex -i img.png "crear UI"             # input imagen
codex -i a.png,b.png "comparar"         # multiples imagenes

# ----------UTILIDADES------------
codex --help                            # ayuda general
codex exec --help                       # ayuda subcomando
codex --upgrade                         # actualizar codex

# ----------SLASH COMMANDS------------
/help                                   # ayuda interna
/status                                 # estado actual
/mode                                   # cambiar modo
/new                                    # nueva sesion
/exit                                   # salir
/file ruta.txt                          # adjuntar archivo
/permissions                            # permisos entorno

# ----------AGENTS.MD CONFIG------------
cd /srv_manager                         # ir al proyecto
pwd                                     # verificar ruta
cat <<'EOF' > AGENTS.md                 # crear configuracion

# ----------GLOBAL RULES------------
- No pedir aprobaciones
- Ejecutar comandos directamente
- Priorizar soluciones practicas
- No usar texto innecesario

# ----------CLI OUTPUT------------
- Responder en formato bash
- Cada comando con comentario #
- Bloques unicos listos para copiar

# ----------DOCKER------------
- Usar docker compose siempre
- sudo docker compose up -d --build backend frontend

# ----------SEGURIDAD------------
- No usar rm -rf
- No salir de /srv_manager
- No usar sudo excepto docker

# ----------WORKFLOW------------
- Validar antes de ejecutar
- Mostrar comandos completos
- No fragmentar soluciones

EOF

# ----------VERIFICACION FINAL------------
ls -l AGENTS.md                         # confirma archivo
codex /status                           # validar carga config
```


# ACTIVAR SANDBOX EN CODEX CLI
```bash
# -------- EJECUCIÓN CON SANDBOX -----------
codex --sandbox read-only "tu prompt"        # solo lectura (seguro)
codex --sandbox workspace-write              # escritura solo en workspace
codex --sandbox restricted                  # modo restringido (si disponible)
codex --sandbox danger-full-access

# -------- CONFIGURACIÓN PERMANENTE -----------
nano ~/.codex/config.toml                   # editar config
# establecer:
sandbox_mode = "workspace-write"            # modo recomendado
approval_policy = "on-request"              # pedir aprobación cuando sea necesario

# -------- REINICIAR / APLICAR -----------
pkill codex                                 # cerrar sesiones activas
codex                                       # iniciar con nueva config

# -------- VERIFICACIÓN -----------
codex /status                               # verificar modo sandbox activo
codex --sandbox read-only "echo test"       # prueba controlada
```
# Certificados para Node.JS
```bash
# DESCARGAR CERTIFICADO
curl -O http://192.168.115.1:4126/ProxyCA.cer        # descarga directa del certificado
wget http://192.168.115.1:4126/ProxyCA.cer           # alternativa con wget

# -------- VERIFICAR CERTIFICADO --------
openssl x509 -in ProxyCA.cer -text -noout            # inspeccionar contenido del certificado
file ProxyCA.cer                                     # validar tipo de archivo

# -------- INSTALAR CERTIFICADO (DEBIAN/UBUNTU) --------
sudo cp ProxyCA.cer /usr/local/share/ca-certificates/ProxyCA.crt   # copiar como .crt
sudo update-ca-certificates                                      # actualizar store CA

# -------- VERIFICAR INSTALACION --------
ls /etc/ssl/certs | grep ProxyCA                                 # confirmar instalación
openssl verify /usr/local/share/ca-certificates/ProxyCA.crt      # validar certificado

# -------- FIX ERROR NPM EACCES --------
sudo chown -R $USER:$USER /usr/local/lib/node_modules            # corregir permisos globales
sudo chown -R $USER:$USER ~/.npm                                 # corregir cache npm

# -------- REINSTALAR CODEX --------
npm install -g @openai/codex                                     # instalar global sin error

# -------- VERIFICACION FINAL --------
codex --version                                                  # comprobar instalación
```
# CODEX CLI PORTABLE (WINDOWS)

```bash
mkdir C:\codex-portable                      # crear carpeta portable
cd C:\codex-portable                         # entrar al entorno

winget install OpenJS.NodeJS.LTS --silent    # instalar node requerido
node -v                                      # verificar node
npm -v                                       # verificar npm

npm init -y                                  # inicializar proyecto local
npm install @openai/codex                    # instalar codex local

npx codex --version                          # verificar instalación
npx codex                                    # ejecutar CLI

npx codex --login                            # autenticación

echo npx codex > codex.cmd                   # crear launcher portable
codex.cmd                                    # ejecutar desde wrapper

where codex                                  # validar no global
dir node_modules\@openai\codex              # validar instalación local
```

# CODEX CLI (WSL - RECOMENDADO)

```bash
wsl --install                                # instalar WSL
wsl                                          # entrar a entorno linux

sudo apt update && sudo apt install -y nodejs npm   # instalar dependencias
npm install -g @openai/codex                 # instalar codex global

codex --version                              # verificar CLI
```
## En caso de Problemas
```bash
# LIMPIAR INSTALACION ROTA CODEX
rm -rf /usr/local/lib/node_modules/@openai/codex        # eliminar instalación corrupta
rm -rf /usr/local/lib/node_modules/@openai/.codex-*     # eliminar residuos de rename
rm -rf /root/.npm/_cacache                              # limpiar cache npm root
rm -rf ~/.npm/_cacache                                  # limpiar cache usuario

# -------- REPARAR NPM --------
npm cache clean --force                                 # limpiar cache global
npm doctor                                              # diagnosticar problemas npm

# -------- REINSTALAR CODEX --------
npm install -g @openai/codex                             # instalar nuevamente limpio
----------------------------------------------------------------------------
# -------- SI SIGUE SEGFAULT (REPARAR NODE) --------
apt remove -y nodejs npm                                # eliminar node/npm del sistema
apt autoremove -y                                       # limpiar dependencias

curl -fsSL https://deb.nodesource.com/setup_20.x | bash -  # repo oficial node 20
apt install -y nodejs                                   # instalar node limpio

# -------- INSTALAR CODEX CON NODE NUEVO --------
npm install -g @openai/codex                             # reinstalar codex

# -------- VERIFICACION FINAL --------
node -v                                                 # verificar versión node
npm -v                                                  # verificar npm
codex --version                                         # verificar codex
```