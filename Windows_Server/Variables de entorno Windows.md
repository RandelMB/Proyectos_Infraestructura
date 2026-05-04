# SETX EN WINDOWS (VARIABLES DE ENTORNO PERSISTENTES)

```bash
# -------- USO BÁSICO -----------
setx MI_VAR "valor"              # crear variable de entorno permanente (usuario)
echo %MI_VAR%                    # mostrar variable (requiere nueva sesión)

# -------- SISTEMA (GLOBAL) -----------
setx MI_VAR "valor" /M           # crear variable global (requiere admin)

# -------- CASO PRÁCTICO OLLAMA -----------
setx OLLAMA_HOST "http://localhost:11434"   # definir endpoint por defecto
echo %OLLAMA_HOST%                          # verificar (nueva terminal)

# -------- VERIFICACIÓN -----------
set MI_VAR                      # ver variables en sesión actual (no incluye setx reciente)
reg query HKCU\Environment     # verificar variables persistentes (usuario)
reg query HKLM\SYSTEM\CurrentControlSet\Control\Session\ Manager\Environment   # global
```
