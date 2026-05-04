

# ----------GESTION DE IMPRESORAS (WINDOWS)------------

Explicación mínima: flujo completo (instalar, eliminar, limpiar, verificar).

```bash
# ----------LISTAR------------
Get-Printer                                           # listar impresoras
Get-PrinterDriver                                     # listar drivers

# ----------INSTALAR POR IP------------
Add-PrinterPort -Name "IP_192.168.1.100" -PrinterHostAddress "192.168.1.100"   # crear puerto TCP/IP
Add-PrinterDriver -Name "NOMBRE_DRIVER"                                        # instalar driver
Add-Printer -Name "IMPRESORA_RED" -PortName "IP_192.168.1.100" -DriverName "NOMBRE_DRIVER"   # crear impresora

# ----------INSTALAR COMPARTIDA------------
Add-Printer -ConnectionName "\\SERVIDOR\IMPRESORA"    # agregar impresora compartida

# ----------ELIMINAR IMPRESORA------------
Remove-Printer -Name "NOMBRE_IMPRESORA"               # eliminar impresora

# ----------ELIMINAR TODAS------------
Get-Printer | Remove-Printer                          # eliminar todas las impresoras

# ----------ELIMINAR DRIVERS------------
Remove-PrinterDriver -Name "NOMBRE_DRIVER"            # eliminar driver especifico
Get-PrinterDriver | Remove-PrinterDriver              # eliminar todos los drivers

# ----------LIMPIAR SPOOLER------------
Stop-Service Spooler                                  # detener spooler
Remove-Item "C:\Windows\System32\spool\PRINTERS\*" -Recurse -Force   # limpiar cola
Start-Service Spooler                                 # iniciar spooler

# ----------REINICIAR SERVICIO------------
Restart-Service Spooler                               # reiniciar servicio

# ----------VERIFICACION------------
Get-Printer                                           # verificar impresoras
Get-PrinterDriver                                     # verificar drivers
Get-Service Spooler                                   # estado del servicio
Test-NetConnection 192.168.1.100 -Port 9100           # prueba conectividad impresora
```