# SINCRONIZAR HORA AUTOMATICAMENTE
```bash
# -------- SERVICIO DE TIEMPO -----------
net stop w32time                 # detener servicio de hora
w32tm /unregister                # desregistrar servicio
w32tm /register                  # registrar servicio nuevamente
net start w32time                # iniciar servicio

# -------- SINCRONIZACION -----------
w32tm /resync                    # forzar sincronizacion inmediata
w32tm /query /status             # verificar estado actual

# -------- CONFIGURAR SERVIDORES NTP -----------
w32tm /config /manualpeerlist:"time.windows.com,0.pool.ntp.org" /syncfromflags:manual /update   # definir servidores NTP
w32tm /resync                    # aplicar sincronizacion

# -------- VERIFICACION -----------
time /t                          # mostrar hora actual
date /t                          # mostrar fecha actual
w32tm /query /peers              # ver servidores configurados
```

# VER SERVIDOR NTP CONFIGURADO
```bash
# -------- CONSULTAR CONFIGURACION -----------
w32tm /query /configuration      # muestra configuracion completa NTP
w32tm /query /peers              # lista servidores NTP configurados
w32tm /query /source             # servidor NTP actualmente en uso

# -------- DETALLE DEL ESTADO -----------
w32tm /query /status             # estado de sincronizacion (incluye source activo)

# -------- VERIFICACION -----------
time /t                          # confirmar hora actual
```