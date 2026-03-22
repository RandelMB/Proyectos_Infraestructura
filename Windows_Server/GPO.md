

Directo: no hay un solo comando mágico que diga “lista de plantillas GPO”, pero sí tienes varias formas **reales y útiles** de verlas.

---

# 🔍 1. Ver GPOs y su contenido (lo más práctico)

```powershell
Get-GPO -All
```

✔ Lista todas las políticas

---

```powershell
Get-GPOReport -All -ReportType Html -Path C:\gpo.html
```

✔ Te muestra TODAS las configuraciones (incluyendo plantillas usadas)

---

# 🧠 2. Ver configuraciones tipo “plantillas administrativas”

```powershell
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\rsop.html
```

✔ Te muestra qué políticas (ADMX) se están aplicando realmente

---

# 📂 3. Ver directamente los archivos de plantillas (ADMX)

Las plantillas están en el sistema, no en PowerShell:

```bash
C:\Windows\PolicyDefinitions\
```

✔ Aquí están los `.admx` (plantillas)  
✔ `.adml` (idiomas)

---

## En dominio (importante)

```bash
\\dominio.local\SYSVOL\dominio.local\Policies\PolicyDefinitions\
```

✔ Este es el **Central Store**  
✔ Aquí se cargan las plantillas para todo el dominio

---

# 🔎 4. Buscar configuraciones específicas dentro de GPO

```powershell
Get-GPOReport -Name "NombreGPO" -ReportType Xml
```

✔ Luego buscas dentro del XML:

- `Software\Policies`
    
- configuraciones ADMX
    

---

# ⚙️ 5. Ver qué políticas están aplicadas (rápido)

```bash
gpresult /r
```

o más completo:

```bash
gpresult /h reporte.html
```

---

# 🧪 6. Método PRO (ver qué plantilla toca qué key)

Usa:

👉 **Process Monitor**

✔ Filtra:

```plaintext
RegSetValue
```

✔ Activas una policy en `gpedit.msc`  
✔ Ves qué key toca realmente

---

# 🔥 Resumen claro

- No existe comando directo para “listar ADMX como políticas”
    
- `Get-GPOReport` → ver configuraciones reales
    
- `gpresult` → ver lo aplicado
    
- `PolicyDefinitions` → ver plantillas
    
- Process Monitor → ver cómo funcionan internamente
    

---

# 🧠 Recomendación práctica

Si quieres trabajar serio:

1. Usa `Get-GPOReport` → para auditoría
    
2. Usa `gpresult` → para troubleshooting
    
3. Usa carpeta `PolicyDefinitions` → para entender plantillas
    

---

Si quieres, te enseño cómo **leer un archivo ADMX y entender exactamente qué registry key modifica cada policy** (eso ya es nivel avanzado real de GPO).

Directo: hay **3 formas reales** de saber qué política usar y cuál es su ruta (registry key). No hay magia, se hace así:

---

# 🔍 1. Método más rápido (GUI + explicación)

1. Abre:
    

```bash
gpedit.msc
```

2. Busca la política (ej: bloquear CMD)
    
3. Ábrela y mira la pestaña:  
    👉 **“Explicación”**
    

✔ Muchas veces te dice:

- qué hace
    
- qué key toca
    
- qué valor usa
    

---

# 🧠 2. Método PRO (100% preciso)

Usa:  
👉 **Process Monitor**

### Pasos:

1. Abre Process Monitor
    
2. Filtra:
    

```plaintext
Operation = RegSetValue
```

3. Abre `gpedit.msc`
    
4. Activa la política
    
5. Mira qué clave se modifica
    

✔ Eso es la **ruta real**

---

# 📂 3. Leer el archivo ADMX (la verdad absoluta)

Las plantillas están aquí:

```bash
C:\Windows\PolicyDefinitions\
```

Busca el `.admx` relacionado (ej: `system.admx`)

Dentro verás algo así:

```xml
<policy name="DisableCMD" ...>
  <registrySettings>
    <registry>
      <key>Software\Policies\Microsoft\Windows\System</key>
      <value name="DisableCMD" type="REG_DWORD" value="1"/>
```

✔ Aquí tienes:

- key exacta
    
- valor
    
- tipo
    

---

# ⚖️ Cómo saber qué política elegir

## Regla clara:

### ✔ Si existe en GPO (gpedit)

→ úsala  
→ es estable y soportada

---

### ❌ Si no existe

→ busca en registry manual  
→ ejemplo:

- USB → `SYSTEM\Services`
    
- cosas avanzadas → `CurrentVersion`
    

---

# 🔥 Flujo correcto (como profesional)

1. Buscas en `gpedit.msc`
    
2. Si existe → perfecto
    
3. Si no:
    
    - buscas en internet la key
        
    - validas con Process Monitor
        

---

# 🧪 Ejemplo real (tu caso)

## Bloquear CMD

✔ Existe en GPO  
→ usar:

```plaintext
HKCU\Software\Policies\Microsoft\Windows\System
DisableCMD = 1
```

---

## Bloquear USB

❌ No es policy real completa  
→ usar:

```plaintext
HKLM\SYSTEM\CurrentControlSet\Services\USBSTOR
Start = 4
```

---

# 🧠 Cómo no equivocarte

- Policies → siempre en `Software\Policies`
    
- Sistema → fuera de ahí
    
- ADMX → define políticas “oficiales”
    

---

# 💥 Resumen sin rodeo

- gpedit = catálogo de políticas
    
- ADMX = definición real
    
- registry = ejecución real
    
- Process Monitor = verdad absoluta
    

---

