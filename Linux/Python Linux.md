
## Instalacion
```sh
sudo apt update
sudo apt install -y python3-pip
pip3 --version
```

### Entorno virtual de Python ( Aísla dependencias )
```sh
python3 -m venv venv             # Crear Entorno Virutal
source venv/bin/activate         # Activa ese entorno virtual
```


### En caso de no tener internet ( En mi caso un firewall me denegaba la conexión a algunas librerías pip)
```sh
# Descargo las dependencias en una carpeta llamada paquetes
python -m pip download flask scapy -d paquetes --only-binary=:all:
python -m pip download python-nmap --no-deps -d paquetes
```