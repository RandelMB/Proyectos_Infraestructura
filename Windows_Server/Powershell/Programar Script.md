


```powershell

# Ver Tareas

start-ScheduledTask -TaskName "limpiar"
Get-ScheduledTask -TaskName "Limpiar" | Format-List *

# 
```




```powershell
# Crear Tarea - Define variables
$TaskName = "LimpiezaAccesosDirectos"
$ScriptPath = "C:\Scripts\limpieza_accesos.ps1"


# Especifica accion 
$Action = New-ScheduledTaskAction `
    -Execute "powershell.exe" `
    -Argument "-ExecutionPolicy Bypass -File `"$ScriptPath`""


$Trigger = New-ScheduledTaskTrigger `
    -Once `
    -At (Get-Date) `
    -RepetitionInterval (New-TimeSpan -Minutes 10) `
    -RepetitionDuration ([TimeSpan]::MaxValue)
```

---

## 4) Ejecutar con privilegios altos (recomendado)

```powershell
$Principal = New-ScheduledTaskPrincipal `
    -UserId "SYSTEM" `
    -LogonType ServiceAccount `
    -RunLevel Highest
```

---

## 5) Registrar la tarea

```powershell
Register-ScheduledTask `
    -TaskName $TaskName `
    -Action $Action `
    -Trigger $Trigger `
    -Principal $Principal `
    -Force
```

---

## 6) Verificar que quedó creada

```powershell
Get-ScheduledTask -TaskName $TaskName
```

---

## 7) Probarla manualmente (opcional)

```powershell
Start-ScheduledTask -TaskName $TaskName
```

---

