# ----------ICACLS BASICO------------

Explicación mínima: Gestiona permisos NTFS desde CMD en Windows.

```bash
icacls archivo.txt                     # ver permisos actuales
icacls carpeta /T                      # ver permisos recursivos
icacls archivo.txt /grant usuario:F    # otorgar control total
icacls archivo.txt /grant usuario:R    # otorgar solo lectura
icacls archivo.txt /deny usuario:W     # denegar escritura
icacls archivo.txt /remove usuario     # eliminar permisos del usuario
icacls archivo.txt /inheritance:e      # habilitar herencia
icacls archivo.txt /inheritance:d      # deshabilitar herencia
icacls archivo.txt /setowner usuario   # cambiar propietario
icacls archivo.txt /reset              # restaurar permisos por defecto
icacls carpeta /grant usuario:F /T     # aplicar permisos recursivamente
icacls archivo.txt /save permisos.txt  # exportar ACL a archivo
icacls . /restore permisos.txt         # restaurar ACL desde archivo
icacls archivo.txt /verify             # verificar integridad de ACL
```
# Permisos de carpetas
```sh
# ===== ARCHIVO id_rsa (SSH) =====
icacls $env:USERPROFILE\.ssh\id_rsa /inheritance:r         # quita herencia
icacls $env:USERPROFILE\.ssh\id_rsa /remove *S-1-5-21-1434609910-3478190980-1902549334-2683758108   # elimina SID desconocido
icacls $env:USERPROFILE\.ssh\id_rsa /grant:r "$($env:USERNAME):(F)"   # da control total solo a tu usuario
icacls $env:USERPROFILE\.ssh\id_rsa /remove:g "Users"     # elimina grupo Users
icacls $env:USERPROFILE\.ssh\id_rsa /remove:g "Authenticated Users"   # elimina usuarios autenticados
icacls $env:USERPROFILE\.ssh\id_rsa /remove:g "Everyone"  # elimina acceso global
icacls $env:USERPROFILE\.ssh\id_rsa                      # verifica permisos

# ===== CARPETAS =====
icacls C:\ruta\carpeta /grant Randel:(F)                  # da permisos a carpeta
icacls C:\ruta\carpeta /grant Randel:(F) /T               # aplica a todo el contenido (recursivo)
/T                                                       # incluye subcarpetas y archivos

icacls C:\ruta\carpeta /inheritance:r                     # quita herencia en carpeta

# ===== EJEMPLO COMPLETO EN CARPETA =====
icacls C:\ruta\carpeta /inheritance:r                     # quita herencia
icacls C:\ruta\carpeta /grant Randel:(F) /T               # permisos recursivos
icacls C:\ruta\carpeta /remove:g "Users"                  # elimina grupo Users

# --------Quitar herencia para quitar usuario desconocido --------#
icacls idKey /inheritance:r                   # quita herencia
icacls idKey /grant:r "$($env:USERNAME):R"    # solo tu usuario puede leer
icacls idKey /remove "Users"                  # elimina acceso general
icacls idKey /remove "Authenticated Users"
```


| Concepto / Acción         | Detalle                                               | Ejemplo / Comando                                                     | Resultado esperado                |
| ------------------------- | ----------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------- |
| Ver permisos (PowerShell) | Comando para ver permisos del archivo `id_rsa`        | `icacls $env:USERPROFILE\.ssh\id_rsa`                                 | Lista de usuarios y permisos      |
| Salida correcta           | Solo usuarios válidos con control total               | `SYSTEM:(F)``Administrators:(F)``Randel:(F)`                          | SSH acepta la clave               |
| Salida incorrecta         | Usuarios genéricos o desconocidos                     | `Users:(R)``Everyone:(R)``Authenticated Users:(R)``UNKNOWN:(R)`       | SSH rechaza la clave              |
| SID desconocido           | Usuario inválido mostrado como SID (`S-1-5-21-...`)   | `S-1-5-21-...:(I)(RX,W)`                                              | SSH lo interpreta como UNKNOWN    |
| Herencia `(I)`            | Permisos heredados de carpeta padre                   | `(I)` en los permisos                                                 | Hace la clave insegura            |
| Regla clave               | Acceso solo para usuario, SYSTEM y Administrators     | —                                                                     | Requisito de OpenSSH              |
| Método gráfico (GUI)      | Ver permisos desde interfaz                           | Click derecho → Propiedades → Seguridad                               | Visualizar accesos fácilmente     |
| Quitar herencia           | Elimina permisos heredados                            | `icacls $env:USERPROFILE\.ssh\id_rsa /inheritance:r`                  | Quita `(I)`                       |
| Eliminar SID desconocido  | Borra usuario inválido                                | `icacls $env:USERPROFILE\.ssh\id_rsa /remove *S-1-5-21-...`           | Elimina UNKNOWN                   |
| Dejar solo tu usuario     | Permiso exclusivo al dueño                            | `icacls $env:USERPROFILE\.ssh\id_rsa /grant:r "$($env:USERNAME):(F)"` | Solo tu usuario con control total |
| Limpiar grupos comunes    | Quitar accesos inseguros                              | `icacls ... /remove:g "Users"``"Authenticated Users"``"Everyone"`     | Mayor seguridad                   |
| Verificación final        | Confirmar permisos correctos                          | `icacls $env:USERPROFILE\.ssh\id_rsa`                                 | Sin SID, sin `(I)`                |
| Estado final esperado     | Configuración correcta para SSH                       | `RANDEL-PC\Randel:(F)` (opcional SYSTEM/Admin)                        | Conexión SSH funciona             |
| Permisos en carpetas      | Tipos de permisos disponibles                         | `(F)` Full control`(M)` Modify`(RX)` Read + Execute                   | Control granular de acceso        |
| Uso de `/T`               | Aplica cambios de forma recursiva                     | `icacls C:\ruta\carpeta /grant Randel:(F) /T`                         | Afecta todo el contenido          |
| Advertencia `/T`          | Impacta todos los archivos y subcarpetas              | —                                                                     | Usar con precaución               |

