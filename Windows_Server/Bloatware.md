# ----------DESACTIVAR BLOATWARE WINDOWS------------

Explicación mínima: Desactiva apps sugeridas y contenido automático.

```bash
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v OemPreInstalledAppsEnabled /t REG_DWORD /d 0 /f      # desactiva apps OEM
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v PreInstalledAppsEnabled /t REG_DWORD /d 0 /f         # desactiva apps preinstaladas
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SilentInstalledAppsEnabled /t REG_DWORD /d 0 /f      # desactiva instalaciones silenciosas
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SoftLandingEnabled /t REG_DWORD /d 0 /f              # desactiva sugerencias iniciales
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f     # desactiva sugerencias en panel

# --------VERIFICACION--------
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager"   # verificar valores
```