
### Instalacion
```sh
sudo apt update
sudo apt install -y python3-pip
pip3 --version
```

#### Entorno virtual de Python ( Aísla dependencias )
```sh
python3 -m venv venv             # Crear Entorno Virutal
source venv/bin/activate         # Activa ese entorno virtual


# CREAR ENTORNO PYTHON (AISLADO)
sudo apt update                                             # actualiza repositorios
sudo apt install python3-venv python3-pip -y                # instala herramientas de Python

python3 -m venv venv                                        # crea entorno virtual (aisla librerías)
source venv/bin/activate                                    # activa el entorno virtual

# ================================
# INSTALAR LIBRERÍAS
# ================================

pip install pysnmp                                         # instala librería SNMP en Python
pip install pysnmp --retries 10 --timeout 100              # reintento con tolerancia a fallos de red

# ================================
# EJECUTAR SCRIPT
# ================================

python3 snmp_extractor.py                                   # ejecuta el script con Python3
python snmp_extractor.py                                    # ejecuta usando python del entorno activo

# ================================
# SALIR DEL ENTORNO VIRTUAL
# ================================

deactivate                                                  # salir del entorno virtual

# ================================
# INSTALACIÓN ALTERNATIVA (SISTEMA)
# ================================

sudo apt install python3-pysnmp4 -y                         # instala pysnmp desde repositorio del sistema

# ================================
# EJECUCIÓN SIN VENV
# ================================

python3 snmp_extractor.py                                   # ejecuta usando librerías del sistema

# ================================
# OCULTAR WARNINGS
# ================================

python3 -W ignore snmp_extractor.py                         # ejecuta ignorando warnings de librerías

# ================================
# DEBUG DE RED (IMPORTANTE)
# ================================

ping -c 3 google.com                                        # prueba conectividad a internet
curl https://pypi.org                                       # prueba acceso a repositorio Python

# ================================
# SNMP DESDE SISTEMA (ALTERNATIVA PRO)
# ================================

sudo apt install snmp snmp-mibs-downloader -y              # instala herramientas SNMP

snmpwalk -v2c -c public 192.168.1.1                        # prueba SNMP completa
snmpget -v2c -c public 192.168.1.1 1.3.6.1.2.1.1.1.0       # consulta un OID específico

# ================================
# WSL (SI LO USAS EN WINDOWS)
# ================================

wsl --install Ubuntu-22.04                                 # instala Ubuntu 22.04 en WSL
```


