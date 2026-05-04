

```sh
# --- DETECCIÓN MS17-010 (EternalBlue)
nmap -p445 --script smb-vuln-ms17-010 <IP>          # Detectar si es vulnerable a MS17-010
nmap -p445 --script smb-protocols <IP>              # Ver versiones SMB habilitadas
nmap -p445 --script smb-security-mode <IP>          # Ver modo de seguridad SMB
nmap -p445 --script smb2-security-mode <IP>         # Seguridad en SMBv2
nmap -p445 -sV <IP>                                # Detectar versión del servicio SMB

# --- ENUMERACIÓN SMB
smbclient -L //<IP> -N                             # Listar recursos sin autenticación
smbclient -L //<IP> -U usuario                     # Listar recursos con usuario
smbclient //<IP>/<share> -N                        # Conectarse a un recurso sin auth
smbclient //<IP>/<share> -U usuario                # Conectarse con credenciales

# --- ENUMERACIÓN AVANZADA
enum4linux <IP>                                    # Enumeración completa SMB (clásico)
enum4linux-ng <IP>                                 # Enumeración moderna mejorada

# --- RECON CON CME (AUDITORÍA)
crackmapexec smb <IP> --shares                     # Ver recursos compartidos
crackmapexec smb <IP> --users                      # Enumerar usuarios
crackmapexec smb <IP> --pass-pol                   # Política de contraseñas
crackmapexec smb <IP> --sessions                   # Sesiones activas

# --- VALIDACIÓN RÁPIDA DE VULNERABILIDAD
nmap -p445 --script smb-vuln-ms17-010 <IP> | grep VULNERABLE   # Filtrar resultado vulnerable

# --- HARDENING / MITIGACIÓN (Windows)
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol   # Ver si SMBv1 está activo
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol  # Deshabilitar SMBv1
Set-SmbServerConfiguration -EnableSMB1Protocol $false          # Desactivar SMBv1 (server)

# --- FIREWALL (Windows)
netsh advfirewall firewall show rule name=all | findstr 445    # Ver reglas relacionadas a SMB
netsh advfirewall firewall add rule name="Block SMB" dir=in action=block protocol=TCP localport=445  # Bloquear puerto 445

# --- MONITOREO / DETECCIÓN (IDS)
grep -i "ETERNALBLUE" /var/log/suricata/fast.log              # Buscar alertas en Suricata
grep -i "MS17-010" /var/log/snort/alert                      # Buscar alertas en Snort
```