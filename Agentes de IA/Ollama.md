
## Orden recomendado de uso
Tu flujo diario debería ser:
```text
1. Variables
2. Start-Process serve
3. ollama list
4. ollama run modelo
5. API local
6. firewall LAN
7. docker/OpenClaw
```
# OLLAMA CLI RUNBOOK (WINDOWS + LINUX)
```bash
# =========================================================
# 1) WINDOWS - INSTALACION
# =========================================================
winget install Ollama.Ollama
ollama --version

# =========================================================
# 2) WINDOWS - CONFIGURACION PERSISTENTE (E: + GPU BAJA)
# =========================================================
setx OLLAMA_MODELS "E:\Ollama\models"
setx OLLAMA_VULKAN "1"
setx OLLAMA_KEEP_ALIVE "0"
setx OLLAMA_NUM_PARALLEL "1"
setx OLLAMA_MAX_LOADED_MODELS "1"
setx OLLAMA_CONTEXT_LENGTH "2048"

# aplicar en sesion actual
$env:OLLAMA_MODELS="E:\Ollama\models"
$env:OLLAMA_VULKAN="1"
$env:OLLAMA_KEEP_ALIVE="0"
$env:OLLAMA_NUM_PARALLEL="1"
$env:OLLAMA_MAX_LOADED_MODELS="1"
$env:OLLAMA_CONTEXT_LENGTH="2048"

# =






# =========================================================
# 3) WINDOWS - SERVICIO / API
# =========================================================
# iniciar API local
Start-Process "ollama" -ArgumentList "serve"

# validar API
curl http://localhost:11434/api/tags
Invoke-WebRequest -UseBasicParsing http://127.0.0.1:11434/api/version

# exponer en LAN (solo si necesitas acceso remoto)
$env:OLLAMA_HOST="0.0.0.0:11434"

# =========================================================
# 4) WINDOWS - GESTION DE MODELOS
# =========================================================
ollama pull jaahas/qwen3-abliterated:4b
ollama list
ollama show jaahas/qwen3-abliterated:4b
ollama rm NOMBRE_MODELO

# dejar solo 1 modelo
# repite rm según lo que tengas
# ejemplo:
# ollama rm llama3
# ollama rm mistral

# =========================================================
# 5) WINDOWS - EJECUCION
# =========================================================
ollama run jaahas/qwen3-abliterated:4b
ollama run jaahas/qwen3-abliterated:4b "analiza logs firewall"
ollama ps
ollama stop jaahas/qwen3-abliterated:4b

# =========================================================
# 6) WINDOWS - MODELOS PERSONALIZADOS
# =========================================================
ollama cp jaahas/qwen3-abliterated:4b redteam-lab
ollama create redteam-lab -f Modelfile

# =========================================================
# 7) WINDOWS - API DIRECTA
# =========================================================
curl http://localhost:11434/api/generate -d "{\"model\":\"jaahas/qwen3-abliterated:4b\",\"prompt\":\"hola\",\"stream\":false}"

curl http://localhost:11434/api/chat -d "{\"model\":\"jaahas/qwen3-abliterated:4b\",\"messages\":[{\"role\":\"user\",\"content\":\"hola\"}]}"

# =========================================================
# 8) WINDOWS - GPU / VULKAN DIAGNOSTICO
# =========================================================
Get-CimInstance Win32_VideoController | Select Name,AdapterRAM,DriverVersion
wmic path win32_VideoController get name,AdapterRAM
dxdiag /t dx.txt
notepad dx.txt
vulkaninfo
tasklist | findstr ollama

# NVIDIA (si aplica)
nvidia-smi
nvidia-smi --query-gpu=name,memory.total --format=csv

# =========================================================
# 9) WINDOWS - TROUBLESHOOTING PUERTO 11434
# =========================================================
netstat -ano | findstr 11434
taskkill /IM ollama.exe /F
netstat -ano | findstr 11434

# iniciar limpio
Start-Process "ollama" -ArgumentList "serve"

# verificar una sola instancia
tasklist | findstr ollama
netstat -ano | findstr 11434

# =========================================================
# 10) WINDOWS - FIREWALL / HARDENING
# =========================================================
netsh advfirewall firewall add rule name="Ollama LAN Only" dir=in action=allow protocol=TCP localport=11434 remoteip=172.16.0.0/16

# prueba LAN
curl http://IP_WINDOWS:11434/api/tags

# contenedor / docker
docker exec -it CONTENEDOR curl http://IP_WINDOWS:11434/api/tags

# =========================================================
# 11) WSL
# =========================================================
export OLLAMA_HOST=http://localhost:11434
curl $OLLAMA_HOST/api/tags
ollama run jaahas/qwen3-abliterated:4b

# =========================================================
# 12) LINUX (DEBIAN/UBUNTU)
# =========================================================
curl -fsSL https://ollama.com/install.sh | sh
ollama --version

sudo systemctl enable ollama
sudo systemctl start ollama
systemctl status ollama

# modelos
ollama pull llama3
ollama list
ollama show llama3
ollama run llama3
ollama rm llama3

# api
curl http://localhost:11434/api/tags
curl http://localhost:11434/api/generate -d '{"model":"llama3","prompt":"hola"}'
```

# CAMBIAR CONTEXTO DE MODELOS
```bash
# -------- VER CONTEXTO DE MODELOS -----------
ollama list                                              # listar modelos instalados
ollama show llama3:8b                                    # ver detalles (buscar "context length")
ollama show llama3:8b --modelfile                        # ver config interna (num_ctx)

# -------- EXTRAER Y MODIFICAR CONTEXTO -----------
ollama show f0rc3ps/nu11secur1tyAIRedTeamLite:latest --modelfile > Modelfile   # exportar config
nano Modelfile                                           # editar archivo (si no hay nano usar notepad)
# cambiar:
# PARAMETER num_ctx 4096 -> PARAMETER num_ctx 32768

# -------- CREAR MODELO CON NUEVO CONTEXTO -----------
ollama create redteam-32k -f Modelfile                   # crear modelo modificado

# -------- VERIFICACION -----------
ollama show redteam-32k                                  # confirmar num_ctx actualizado
ollama run redteam-32k                                   # probar ejecución
```

