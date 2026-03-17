

## **Forma correcta (recomendada)**

Usa el instalador oficial. Evita problemas y versiones viejas.

### 1. Actualiza el sistema

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Instala dependencias básicas

```bash
sudo apt install -y curl gnupg2
```

### 3. Descarga el instalador de Metasploit

```bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfupdate
```

### 4. Da permisos y ejecuta

```bash
chmod +x msfupdate
sudo ./msfupdate
```

Esto instala:

- Metasploit    
- PostgreSQL    
- Todo lo necesario para que funcione bien
    

---

## **Inicializar Metasploit**

Después de instalar:

```bash
msfconsole
```

La primera vez tardará un poco. Es normal.

Si quieres verificar la base de datos:

```bash
msfdb status
```

Si no está activa:

```bash
sudo msfdb init
```

---
## **Forma NO recomendada (repositorios de Debian)**

```bash
sudo apt install metasploit-framework
```

Problema:
- Versión vieja    
- Falta módulos    
- Da errores con exploits nuevos   

Útil solo para pruebas básicas.

---
## **Conclusión clara**
- Para aprender o trabajar en serio → **instalador oficial**    
- Para jugar rápido → repositorio (no ideal)
