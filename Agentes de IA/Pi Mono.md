# Instalacion
```bash
# INSTALAR PI MONO (CLI IA)
apt update && apt install -y git python3 python3-venv python3-pip curl   # dependencias
git clone https://github.com/pimono-ai/pi-mono.git                       # clonar repo oficial
cd pi-mono                                                               # entrar al proyecto

python3 -m venv venv                                                     # crear entorno virtual
source venv/bin/activate                                                 # activar entorno
pip install -r requirements.txt                                          # instalar dependencias

# -------- CONFIGURACION --------
cp .env.example .env                                                     # copiar plantilla config
nano .env                                                                # editar API keys (OpenRouter/OpenAI/etc)

# -------- EJECUTAR AGENTE --------
python main.py                                                           # iniciar agente CLI

# -------- USO BASICO --------
python main.py --help                                                    # ver opciones
python main.py "analiza este log"                                        # prompt directo

# -------- EJECUCION EN BACKGROUND --------
nohup python main.py > pi-mono.log 2>&1 &                                # ejecutar en background

# -------- ACTUALIZAR --------
git pull                                                                 # actualizar código
source venv/bin/activate && pip install -r requirements.txt              # actualizar deps

# -------- VERIFICACION --------
ps aux | grep main.py                                                    # proceso activo
tail -f pi-mono.log                                                      # logs agente
```

# Configurar Api Key
```bash
# UBICACION DOCUMENTACION
cat /usr/local/lib/node_modules/@mariozechner/pi-coding-agent/docs/providers.md   # ver providers soportados

# -------- CONFIGURAR API GROQ --------
export GROQ_API_KEY="TU_API_KEY_GROQ"                 # exportar API key Groq
echo 'export GROQ_API_KEY="TU_API_KEY_GROQ"' >> ~/.bashrc   # persistir variable

# -------- CONFIGURAR PROVIDER EN PI MONO --------
export MODEL_PROVIDER="groq"                          # seleccionar provider groq
export MODEL_NAME="llama3-70b-8192"                   # modelo recomendado groq
echo 'export MODEL_PROVIDER="groq"' >> ~/.bashrc
echo 'export MODEL_NAME="llama3-70b-8192"' >> ~/.bashrc

# -------- EJECUTAR AGENTE --------
pi-coding-agent                                       # iniciar agente con groq

# -------- DEBUG / VALIDACION --------
env | grep -E "GROQ|MODEL"                            # validar variables
pi-coding-agent --verbose                             # ejecutar en modo debug

# -------- TEST RAPIDO --------
echo "hola mundo" | pi-coding-agent                   # prueba básica

# -------- VERIFICACION FINAL --------
pi-coding-agent --help                                # confirmar funcionamiento
```

# Reemplazar api de groq
```bash
# REEMPLAZAR API KEY GROQ (SESION ACTUAL)
unset GROQ_API_KEY                                     # eliminar key actual en memoria
export GROQ_API_KEY="NUEVA_API_KEY_GROQ"               # asignar nueva API key

# -------- ACTUALIZAR PERMANENTE --------
sed -i 's/^export GROQ_API_KEY=.*/export GROQ_API_KEY="NUEVA_API_KEY_GROQ"/' ~/.bashrc   # reemplazar en bashrc
source ~/.bashrc                                       # recargar configuración

# -------- VALIDAR CONFIGURACION --------
env | grep GROQ_API_KEY                                # verificar variable cargada

# -------- TEST AGENTE --------
pi-coding-agent --verbose                              # ejecutar con nueva API

# -------- VERIFICACION FINAL --------
echo $GROQ_API_KEY                                     # confirmar valor activo
```