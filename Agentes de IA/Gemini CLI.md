# Instalacion
```bash
# INSTALAR NODEJS EN WINDOWS (POWERSHELL ADMIN)
winget install OpenJS.NodeJS.LTS                 # instalar Node.js LTS
node -v                                          # verificar node
npm -v                                           # verificar npm

# ACTUALIZAR NPM
npm install -g npm@latest                        # actualizar npm global

# INSTALAR GEMINI CLI
npm install -g @google/gemini-cli                # instalar CLI Gemini

# VERIFICAR
gemini --version                                 # validar instalación
npm list -g --depth=0 | findstr gemini           # confirmar paquete
```
# Get Start
```bash
# CONFIGURAR AUTENTICACIÓN
setx GEMINI_API_KEY "TU_API_KEY"                # definir API key en Windows
echo %GEMINI_API_KEY%                           # verificar variable

# INICIAR CLI
gemini                                          # abrir modo interactivo
gemini "Hola mundo"                             # prompt directo rápido

# USO BÁSICO
gemini "explica redes neuronales"               # consulta simple
gemini -m gemini-1.5-pro "analiza este texto"   # usar modelo específico
gemini -t 0.7 "generar ideas de negocio"        # ajustar temperatura

# ENTRADA DESDE ARCHIVO
type archivo.txt | gemini                       # enviar archivo como input
gemini < archivo.txt                            # alternativa stdin

# GUARDAR SALIDA
gemini "crear script bash backup" > script.sh   # guardar resultado

# MODO CHAT CONTINUO
gemini chat                                     # sesión interactiva persistente

# DEBUG / VERBOSE
gemini -v "test"                                # salida detallada

# VERIFICACIÓN
gemini --help                                   # ver opciones disponibles
gemini --version                                # validar instalación
```
# LINUX
```bash
# INSTALAR GEMINI CLI (OFICIAL GOOGLE)
apt update && apt install -y curl nodejs npm            # instalar dependencias
npm install -g @google/generative-ai-cli               # instalar CLI Gemini

# -------- CONFIGURAR API KEY --------
export GOOGLE_API_KEY="TU_API_KEY_GEMINI"              # exportar clave API
echo 'export GOOGLE_API_KEY="TU_API_KEY_GEMINI"' >> ~/.bashrc  # persistir variable

# -------- USO BASICO --------
gemini --help                                          # ver ayuda
gemini "explica que es un firewall"                    # prueba rápida

# -------- VERIFICACION FINAL --------
gemini --version                                       # verificar instalación
echo $GOOGLE_API_KEY                                   # confirmar variable
```
# START
```bash
# DIAGNOSTICO BASE (NODE/NPM)
node -v                                                # verificar versión node
npm -v                                                 # verificar versión npm
which node                                             # ruta binario node
which npm                                              # ruta binario npm
npm doctor                                             # diagnóstico automático

# -------- LIMPIEZA GLOBAL NPM --------
npm cache clean --force                                # limpiar cache corrupto
rm -rf ~/.npm/_cacache                                 # eliminar cache usuario
rm -rf /root/.npm/_cacache                             # eliminar cache root
rm -rf /usr/local/lib/node_modules/*                   # limpiar módulos globales rotos

# -------- REPARAR PERMISOS --------
chown -R $USER:$USER ~/.npm                            # corregir permisos usuario
chown -R $USER:$USER /usr/local/lib/node_modules       # corregir global
chmod -R 755 /usr/local/lib/node_modules               # permisos ejecución

# -------- REINSTALAR NODE LIMPIO --------
apt remove -y nodejs npm                               # eliminar instalación actual
apt autoremove -y                                      # limpiar dependencias
rm -rf /usr/local/lib/node_modules                     # borrar restos globales

curl -fsSL https://deb.nodesource.com/setup_20.x | bash -  # repo oficial node 20
apt install -y nodejs                                  # instalar node limpio

# -------- REINSTALAR GEMINI CLI --------
npm install -g @google/generative-ai-cli               # instalar CLI Gemini

# -------- VERIFICAR VARIABLES --------
env | grep GOOGLE_API_KEY                              # confirmar API key
echo $PATH                                             # validar PATH correcto

# -------- DEBUG EJECUCION --------
npm config get prefix                                  # verificar ruta global npm
ls -la $(npm config get prefix)/bin                    # verificar binarios instalados

# -------- TEST FINAL --------
gemini --version                                       # verificar CLI
gemini "ping"                                          # prueba funcional
```