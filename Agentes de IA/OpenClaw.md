---
tags:
  - ia
---
damhttp://127.0.0.1:11434/api/version

# CONFIGURAR API LOCAL EN OPENCLAW (OLLAMA)

```bash
# -------- INICIAR SERVIDOR OLLAMA -----------
ollama serve                               # iniciar API local (puerto 11434)
curl http://localhost:11434/api/tags       # verificar API activa

# -------- DEFINIR ENDPOINT EN OPENCLAW -----------
export OPENCLAW_API_BASE="http://172.16.0.151:11434"   # endpoint API local
export OPENCLAW_MODEL="llama3"                      # modelo a usar

# -------- PRUEBA DE CONEXIÓN -----------
curl http://localhost:11434/api/generate -d '{"model":"llama3","prompt":"test"}'   # test API

# -------- EJECUTAR OPENCLAW -----------
openclaw --api $OPENCLAW_API_BASE --model $OPENCLAW_MODEL   # iniciar cliente

# -------- CONFIGURACIÓN PERMANENTE -----------
nano ~/.bashrc                          # editar config
# agregar:
# export OPENCLAW_API_BASE="http://localhost:11434"
# export OPENCLAW_MODEL="llama3"
source ~/.bashrc                        # aplicar cambios

# -------- DEBUG -----------
export OPENCLAW_DEBUG=1                 # activar logs
openclaw --api $OPENCLAW_API_BASE       # ver errores
```
# CONECTAR API LOCAL A CONTENEDOR DOCKER (OPENCLAW)

```bash
# -------- VERIFICAR API LOCAL -----------
curl http://IP_SERVIDOR_API:11434/api/tags   # validar acceso desde red

# -------- EXPONER API EN TODAS LAS INTERFACES -----------
setx OLLAMA_HOST "0.0.0.0:11434"             # permitir acceso externo
taskkill /IM ollama.exe /F                   # reiniciar servicio
ollama serve                                # levantar API pública

# -------- ABRIR PUERTO EN FIREWALL (WINDOWS) -----------
netsh advfirewall firewall add rule name="Ollama 11434" dir=in action=allow protocol=TCP localport=11434

# -------- PROBAR DESDE SERVIDOR DOCKER -----------
curl http://IP_SERVIDOR_API:11434/api/tags   # validar conectividad remota

# -------- CONFIGURAR DOCKER (OPENCLAW) -----------
docker run -d \
-e OLLAMA_BASE_URL=http://IP_SERVIDOR_API:11434 \
--name openclaw \
--restart unless-stopped \
openclaw-image                              # conectar contenedor a API

# -------- SI YA EXISTE CONTENEDOR -----------
docker stop openclaw                        # detener contenedor
docker rm openclaw                          # eliminar
docker run -d -e OLLAMA_BASE_URL=http://IP_SERVIDOR_API:11434 --name openclaw openclaw-image

# -------- VERIFICACIÓN -----------
docker logs openclaw                        # revisar conexión a API
curl http://IP_SERVIDOR_API:11434/api/tags  # confirmar respuesta válida
```

# Validar estado de Openclaw
```bash
# VALIDAR HEALTH OPENCLAW
sudo -n /usr/local/sbin/openclaw_n8n_safe openclaw health   # estado del gateway

# LISTAR MODELOS (CLI AISLADO)
cd /opt/stacks/openclaw
docker compose run --rm openclaw-cli models list   # debe mostrar 1 modelo

# PROBAR AGENTE (CLI DIRECTO)
docker compose run --rm openclaw-cli agent \
--agent ciberseguridad-es \
--message "hola"   # prueba directa sin canal externo

# DEBUG SI SE CUELGA (NO INTERACTIVO)
docker compose run --rm -T openclaw-cli agent \
--agent ciberseguridad-es \
--message "test"   # desactiva TTY

# PROBAR API INTERNA (SIN CLI)
curl -X POST http://localhost:PUERTO/api/chat \
-H "Content-Type: application/json" \
-d '{"agent":"ciberseguridad-es","message":"hola"}'   # prueba directa API

# VERIFICAR CONECTIVIDAD A OLLAMA DESDE CONTENEDOR
docker exec -it CONTENEDOR curl http://172.16.0.251:11434/api/tags   # listar modelos
docker exec -it CONTENEDOR curl -X POST http://172.16.0.251:11434/api/generate \
-d '{"model":"jaahas/qwen3-abliterated:4b","prompt":"ping","stream":false}'   # inferencia

# LOGS OPENCLAW (CRITICO)
docker logs CONTENEDOR | tail -n 100         # últimas peticiones
docker logs CONTENEDOR | grep -i error       # errores

# VALIDAR CONFIG ACTIVA
docker exec -it CONTENEDOR cat /app/config/*.json | grep -E "ollama|172.16.0.251"   # confirmar provider

# POSIBLE FALLA: TIMEOUT / STREAM
# probar sin streaming si aplica
docker exec -it CONTENEDOR env | grep -i stream   # verificar flags

# VERIFICACION FINAL
echo "OK si CLI responde y curl API responde"   # resultado esperado
```

# OPENCLAW SETUP 

```bash
# -------- DEPENDENCIAS -----------
sudo apt update                                      # actualizar repositorios
sudo apt install -y git curl docker.io docker-compose # instalar dependencias

# -------- DOCKER -----------
sudo systemctl enable docker                         # habilitar docker
sudo systemctl start docker                          # iniciar servicio
sudo usermod -aG docker $USER                        # agregar usuario a grupo
newgrp docker                                        # aplicar cambios

# -------- VERIFICACIÓN DOCKER -----------
docker --version                                     # verificar docker
docker compose version                               # verificar compose

# -------- INSTALACIÓN OPENCLAW -----------
git clone https://github.com/openclaw/openclaw.git   # clonar repositorio
cd openclaw                                          # entrar al proyecto
chmod +x docker-setup.sh                             # dar permisos
./docker-setup.sh                                    # instalar contenedores

# -------- ALTERNATIVA MANUAL -----------
docker compose up -d                                 # levantar servicios

# -------- VERIFICACIÓN -----------
docker ps                                            # contenedores activos
curl -fsS http://127.0.0.1:18789/healthz && echo OK  # healthcheck

# -------- LOGS Y ACCESO -----------
docker logs -f openclaw                              # ver logs
docker exec -it openclaw bash                        # entrar contenedor

# -------- TOKEN -----------
cat /home/node/openclaw/openclaw.json                # obtener token

# -------- DISPOSITIVOS -----------
node /home/node/openclaw/cli.js devices list --token TU_TOKEN        # listar dispositivos
node /home/node/openclaw/cli.js devices approve ID --token TU_TOKEN  # aprobar dispositivo

# -------- CONTROL -----------
docker restart openclaw                              # reiniciar contenedor
docker compose down                                  # detener servicios
docker system prune -a -f                            # limpiar docker

# -------- ACCESO WEB -----------
echo "http://localhost:18789"                        # UI
echo "http://localhost:18789/#token=TU_TOKEN"        # UI autenticada

# -------- TÚNEL REMOTO -----------
ssh -N -L 18789:127.0.0.1:18789 root@IP_REMOTA      # túnel SSH

# -------- CLI OPENCLAW -----------
openclaw version                                     # versión
openclaw help                                        # ayuda

# -------- GATEWAY -----------
openclaw gateway run                                 # iniciar gateway
openclaw gateway stop                                # detener gateway
openclaw gateway status                              # estado gateway

# -------- DASHBOARD -----------
openclaw dashboard                                   # generar URL dashboard

# -------- DEVICES -----------
openclaw devices list                                # listar
openclaw devices pair                                # emparejar
openclaw devices unpair                              # eliminar
openclaw devices info                                # información

# -------- MENSAJES -----------
openclaw send text                                   # enviar texto
openclaw send image                                  # enviar imagen
openclaw send file                                   # enviar archivo

# -------- LOGS -----------
openclaw logs                                        # ver logs
openclaw logs --follow                               # tiempo real

# -------- CONFIG -----------
openclaw init                                        # inicializar
openclaw config                                      # editar config

# -------- TOKENS -----------
openclaw token create                                # crear token
openclaw token list                                  # listar tokens
openclaw token revoke                                # revocar token

# -------- MODELOS -----------
openclaw models list                                 # listar modelos
openclaw models status                               # estado modelos
openclaw models set ollama/qwen2.5-coder:7b          # definir modelo
openclaw models scan                                 # escanear modelos

# -------- ALIASES -----------
openclaw models aliases list                         # listar alias
openclaw models aliases add qwen qwen2.5-coder:7b    # crear alias
openclaw models aliases remove qwen                  # eliminar alias

# -------- AUTH MODELOS -----------
openclaw models auth list                            # listar credenciales
openclaw models auth add                             # agregar credencial
openclaw models auth remove                          # eliminar credencial

# -------- FALLBACKS -----------
openclaw models fallbacks list                       # listar fallback
openclaw models fallbacks add llama3                 # agregar fallback
openclaw models fallbacks remove llama3              # eliminar fallback

# -------- IMAGE FALLBACKS -----------
openclaw models image-fallbacks list                 # listar
openclaw models image-fallbacks add modelo           # agregar
openclaw models image-fallbacks remove modelo        # eliminar

# -------- AGENTE -----------
openclaw models --agent ID list                      # listar por agente
openclaw models --agent ID set llama3                # asignar modelo
```
# AUMENTAR VENTANA DE CONTEXTO 
```bash
# -------- BUSQUEDA CONFIG REAL EN CONTENEDOR -----------
docker exec -it openclaw-openclaw-gateway-1 sh            # entrar al contenedor
grep -R "16000" -n /app 2>/dev/null                      # buscar valor dentro del contenedor
grep -R "context" -n /app --exclude-dir=node_modules     # buscar contexto sin ruido
grep -R "modelsConfig" -n /app 2>/dev/null               # ubicar referencia exacta

# -------- IDENTIFICAR VOLUMEN / ORIGEN -----------
docker inspect openclaw-openclaw-gateway-1 | grep -i mount -A 20   # ver volúmenes montados
docker inspect openclaw-openclaw-gateway-1 | grep -i source        # ruta real en host

# -------- BUSCAR EN HOST (FUERA DEL CONTENEDOR) -----------
grep -R "16000" -n /var/lib/docker/volumes 2>/dev/null   # buscar en volúmenes docker
grep -R "modelsConfig" -n /var/lib/docker/volumes 2>/dev/null  # ubicar archivo config real

# -------- EDITAR CONFIG -----------
nano /ruta/encontrada/modelsConfig.json                  # editar archivo real
# cambiar valor:
# "context": 16000 -> 8192

# -------- REINICIAR SERVICIO -----------
docker restart openclaw-openclaw-gateway-1               # aplicar cambios

# -------- VERIFICACION -----------
docker logs openclaw-openclaw-gateway-1 | grep -i context   # confirmar nuevo valor cargado
```

```bash
# -------- PATCH DIRECTO EN DIST -----------
docker exec -it openclaw-openclaw-gateway-1 sh            # entrar al contenedor
cd /app                                                   # ir al root de la app
grep -n "16000" dist/*.js                                 # ubicar línea exacta
nano dist/runner-BfRYetN-.js                              # editar archivo compilado
# cambiar "16000" -> "8192"

# -------- REINICIO CONTENEDOR -----------
exit                                                      # salir del contenedor
docker restart openclaw-openclaw-gateway-1                # aplicar cambios

# -------- VERIFICACION LOGS -----------
docker logs -f openclaw-openclaw-gateway-1 | grep -i context   # validar que no aparezca error

# -------- PATCH AUTOMATICO (PERSISTENTE) -----------
docker exec -it openclaw-openclaw-gateway-1 sh -c "sed -i 's/16000/8192/g' /app/dist/*.js"   # parche automático rápido
docker restart openclaw-openclaw-gateway-1                # reiniciar con parche aplicado

# -------- VALIDAR PARCHE -----------
docker exec -it openclaw-openclaw-gateway-1 sh -c "grep -R '8192' /app/dist"   # confirmar cambio aplicado
```
# AGREGAR MODELO
```bash
# -------- REGISTRAR MODELO EN OPENCLAW -----------
openclaw models list                                      # ver modelos detectados
openclaw models set ollama/redteam-32k                    # establecer como modelo principal

# -------- AGREGAR A FALLBACK -----------
openclaw models fallbacks add ollama/redteam-32k          # añadir como fallback
openclaw models fallbacks list                            # verificar lista

# -------- FORZAR DETECCION (SI NO APARECE) -----------
openclaw models scan                                      # escanear modelos disponibles
openclaw models list                                      # confirmar que aparece

# -------- VERIFICACION FINAL -----------
openclaw models status                                    # validar modelo activo y fallback
```

# AGREGAR MODELOS A FALBACK
```bash
# -------- AGREGAR MODELO AL FALLBACK -----------
openclaw models fallbacks add ollama/redteam-32k          # agregar modelo con mayor contexto

# -------- VERIFICAR LISTA -----------
openclaw models fallbacks list                            # confirmar que fue agregado

# -------- OPCIONAL: LIMPIAR Y REORDENAR -----------
openclaw models fallbacks clear                           # limpiar lista actual
openclaw models fallbacks add ollama/redteam-32k          # poner como principal fallback
openclaw models fallbacks add ollama/H4cker/hacker-ai:latest   # agregar secundarios

# -------- VERIFICACION FINAL -----------
openclaw models status                                    # ver estado completo de modelos
```
# Organizar modelos
```bash
# UBICAR CONFIG DE MODELOS
cd /opt/stacks/openclaw                     # ir al stack
grep -Ri "models" .                         # localizar archivo de config

# EDITAR CONFIG PRINCIPAL (ELIMINAR OTROS MODELOS)
nano config/models.json                     # abrir config (ruta típica)
# DEJAR SOLO:
# {
#   "providers": {
#     "ollama": {
#       "baseUrl": "http://172.16.0.251:11434",
#       "models": [
#         "jaahas/qwen3-abliterated:4b"
#       ]
#     }
#   }
# }

# ELIMINAR PROVIDERS NO USADOS
sed -i '/groq/d' config/models.json        # eliminar groq
sed -i '/google/d' config/models.json      # eliminar google
sed -i '/qwen2.5-coder/d' config/models.json   # eliminar modelo viejo

# VALIDAR CONFIG LIMPIA
cat config/models.json                     # revisar solo ollama activo

# REINICIAR STACK
docker compose down                        # bajar servicios
docker compose up -d                       # levantar limpio

# VERIFICAR MODELOS
docker compose run --rm openclaw-cli models list   # debe mostrar solo 1

# VALIDAR AGENTE
docker compose run --rm openclaw-cli agent \
--agent ciberseguridad-es \
--message "hola"                           # test agente

# LIMPIAR CACHE (SI SIGUEN APARECIENDO)
rm -rf /home/node/.openclaw/*              # limpiar estado local
docker compose restart                    # reiniciar

# VERIFICACION FINAL
docker compose run --rm openclaw-cli models list | grep qwen3   # confirmar único modelo
echo "OK si solo aparece 1 modelo"        # resultado esperado
```

## Ollama Linux
```
# -------- DEBUG OPENCLAW -----------
nano /ruta/openclaw/config/models.json               # editar config modelos
grep -r "minContext" /ruta/openclaw                  # buscar límites
docker exec -it openclaw bash                        # entrar contenedor
grep -r "16000" /app                                # inspección interna
```

```bash
# VALIDAR ARCHIVO HEARTBEAT
ls -l /home/node/.openclaw/workspace/HEARTBEAT.md   # verificar existencia exacta

# LEER CONTENIDO (SI EXISTE)
cat /home/node/.openclaw/workspace/HEARTBEAT.md     # mostrar contenido completo

# RESPUESTA AUTOMATICA SI NO HAY TAREAS
grep -q . /home/node/.openclaw/workspace/HEARTBEAT.md && echo "REVISAR CONTENIDO" || echo "HEARTBEAT_OK"   # lógica básica

# CREAR ARCHIVO SI NO EXISTE
mkdir -p /home/node/.openclaw/workspace             # asegurar ruta
touch /home/node/.openclaw/workspace/HEARTBEAT.md   # crear archivo vacío

# PERMISOS
chown node:node /home/node/.openclaw/workspace/HEARTBEAT.md   # ownership correcto
chmod 644 /home/node/.openclaw/workspace/HEARTBEAT.md         # permisos lectura

# DEBUG OPENCLAW
pwd                                          # validar contexto actual
whoami                                       # validar usuario
env | grep -i openclaw                       # variables entorno relacionadas

# VERIFICACION FINAL
ls -lah /home/node/.openclaw/workspace       # validar archivo presente
cat /home/node/.openclaw/workspace/HEARTBEAT.md   # confirmar contenido
```

# Telegram Bot

```bash
# VERIFICAR TOKEN TELEGRAM
echo $BOT_TOKEN                           # validar variable cargada
curl https://api.telegram.org/bot$BOT_TOKEN/getMe   # probar token (debe responder OK)

# EXPORTAR TOKEN CORRECTO
export BOT_TOKEN="TOKEN_CORRECTO"         # asignar token válido
echo 'export BOT_TOKEN="TOKEN_CORRECTO"' >> ~/.bashrc   # persistente
source ~/.bashrc                          # recargar entorno

# PROBAR DELETE WEBHOOK
curl -X POST https://api.telegram.org/bot$BOT_TOKEN/deleteWebhook   # eliminar webhook

# VERIFICAR ESTADO WEBHOOK
curl https://api.telegram.org/bot$BOT_TOKEN/getWebhookInfo   # revisar estado actual

# CONFIGURAR WEBHOOK (OPCIONAL)
curl -X POST "https://api.telegram.org/bot$BOT_TOKEN/setWebhook" \
-d "url=https://tu_dominio/webhook"        # establecer webhook

# USAR LONG POLLING (ALTERNATIVA)
curl https://api.telegram.org/bot$BOT_TOKEN/getUpdates   # recibir mensajes sin webhook

# DEBUG OPENCLAW
env | grep -i token                     # verificar variables
printenv | grep BOT                     # confirmar export

# VERIFICACION FINAL
curl https://api.telegram.org/bot$BOT_TOKEN/getMe | grep username   # validar bot activo
```
# Rutas
```
  OpenClaw

  - config general: /root/.openclaw/openclaw.json
  - prompt/base del agente principal:
      - /root/.openclaw/workspace/AGENTS.md
      - /root/.openclaw/workspace/IDENTITY.md
      - /root/.openclaw/workspace/TOOLS.md
      - /root/.openclaw/workspace/USER.md
  - agente separado ciberseguridad-es:
      - /root/.openclaw/agents/ciberseguridad-es/workspace/AGENTS.md
      - /root/.openclaw/agents/ciberseguridad-es/workspace/IDENTITY.md
```