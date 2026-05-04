
```python
import requests
import time

# Lista de IPs a intentar
ips = ["192.168.1.1", "192.168.1.2", "192.168.1.3", "192.168.1.4"]

# Credenciales de login
username = "usuario"
password = "contraseña"

# URL de login (ajusta según el servicio)
login_url = "http://ejemplo.com/login"

# Función para intentar login
def intentar_login(ip):
    try:
        # Reemplaza con el método de autenticación real (por ejemplo, POST)
        response = requests.post(
            f"{login_url}?ip={ip}",
            data={"username": username, "password": password},
            timeout=5
        )
        if response.status_code == 200:
            print(f"✅ Login exitoso en {ip}")
        else:
            print(f"❌ Error en {ip}: {response.status_code}")
    except Exception as e:
        print(f"❌ Error en {ip}: {str(e)}")

# Ejecutar intentos
for ip in ips:
    intentar_login(ip)
    time.sleep(1)  # Espera entre intentos
```