# GPUPDATE COMANDOS
```bash
gpupdate                     # actualizar políticas de usuario y equipo
gpupdate /force              # forzar reaplicación completa de todas las GPO
gpupdate /target:user        # actualizar solo políticas de usuario
gpupdate /target:computer    # actualizar solo políticas de equipo
gpupdate /wait:0             # ejecutar sin esperar finalización
gpupdate /wait:60            # esperar máximo 60 segundos
gpupdate /logoff             # cerrar sesión si es requerido por la GPO
gpupdate /boot               # reiniciar si es requerido por la GPO
gpupdate /sync               # aplicar políticas de forma síncrona (inicio/logon)
```
# VERIFICACION
```bash
gpresult /r                  # resumen de GPO aplicadas
gpresult /h reporte.html     # generar reporte HTML detallado
rsop.msc                     # ver políticas resultantes (GUI)
```
# VALIDAR TOKEN Y GRUPOS ACTUALES
```bash
whoami /groups                 # listar grupos en el token actual
whoami /user                   # confirmar usuario actual
echo %USERNAME%                # validar contexto de sesión
```
# FORZAR NUEVO TOKEN SIN REINICIAR
```bash
runas /user:DOMINIO\usuario cmd   # abrir nueva sesión con token actualizado
runas /netonly /user:DOMINIO\usuario cmd  # autenticación contra dominio sin cambiar contexto local
```
# LIMPIAR CREDENCIALES Y TICKETS
```bash
klist purge                    # eliminar tickets Kerberos actuales
cmdkey /list                   # ver credenciales almacenadas
cmdkey /delete:TERMSRV/host    # eliminar credencial específica RDP
```
# VALIDAR ACCESO A RECURSOS CON NUEVO TOKEN
```bash
net use \\servidor\recurso /user:DOMINIO\usuario  # probar acceso con nuevos grupos
dir \\servidor\recurso                           # validar permisos efectivos
```
# REINICIAR PROCESOS DEPENDIENTES (SI APLICA)
```bash
taskkill /IM explorer.exe /F   # cerrar shell actual
start explorer.exe            # reiniciar sesión gráfica (nuevo contexto parcial)
iisreset                      # reiniciar IIS si aplica a permisos web
net stop servicio && net start servicio  # reiniciar servicio dependiente de grupos
```
