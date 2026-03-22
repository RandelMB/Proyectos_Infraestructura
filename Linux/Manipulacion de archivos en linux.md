## Visualizacion de archivos
```python
#-------------- tail (logs) -----------------

tail -n 50 archivo.log        # Muestra las últimas 50 líneas del archivo
tail -f archivo.log           # Sigue el archivo en tiempo real (logs en vivo)
tail -F archivo.log           # Sigue el archivo incluso si rota (logrotate)
tail -n +100 archivo.log      # Muestra desde la línea 100 hasta el final
tail -f app.log error.log     # Muestra múltiples archivos en tiempo real
tail -v -f *.log              # Muestra varios logs con encabezado de archivo
tail -f -s 2 archivo.log      # Actualiza cada 2 segundos
tail -c 500 archivo.log       # Muestra los últimos 500 bytes
tail -f archivo.log | grep ERROR   # Filtra errores en tiempo real
less +F archivo.log           # Alternativa a tail -f con scroll


#-------------- grep ------------------

grep "palabra" archivo.txt         # Muestra líneas que contienen la palabra
grep -i "error" logs.txt           # Ignora mayúsculas/minúsculas
grep -n "error" logs.txt           # Muestra número de línea
grep -E "error|fail|warning" logs.txt   # Busca múltiples palabras
grep -v "error" logs.txt           # Excluye líneas con esa palabra
grep -w "root" archivo.txt         # Coincidencia exacta
grep -c "error" logs.txt           # Cuenta coincidencias
grep -A 3 "error" logs.txt         # Muestra 3 líneas después
grep -B 3 "error" logs.txt         # Muestra 3 líneas antes
grep -i "error" /var/log/syslog    # Buscar errores en logs del sistema

# -----------------------Sed ----------------------------

sed 's/old/new/g'        # reemplazar
sed '/pattern/d'         # eliminar líneas
sed -n '/pattern/p'      # filtrar
sed -i                  # editar archivo directamente

# ----------- útiles ---------------------------

awk '/error/ {print}' archivo.txt       # Filtra líneas que contienen "error"
awk '{print $1}' archivo.txt            # Muestra la primera columna
cut -d":" -f1 /etc/passwd              # Extrae la primera columna separada por :
sort archivo.txt                        # Ordena líneas
sort archivo.txt | uniq                 # Elimina duplicados
head -10 archivo.txt                    # Muestra primeras 10 líneas
tail -10 archivo.txt                    # Muestra últimas 10 líneas


# ----------- útiles2 ---------------------------

grep "texto" archivo.txt                # Buscar texto
awk '{print $1}' archivo.txt            # Procesar datos
sed 's/error/ok/g' archivo.txt          # Reemplazar texto
cut -d":" -f1 archivo.txt              # Extraer columnas
```


## comandos para manipular archivos y directorios
```bash
#// 🔹 LISTAR
ls // Lista archivos y carpetas  
ls -l // Lista detallada (permisos, tamaño, fecha)  
ls -a // Incluye ocultos  
ls -lh // Tamaños en formato legible  
ls -R // Lista recursiva  

// 🔹 CREAR
touch archivo.txt // Crea archivo vacío  
mkdir carpeta // Crea carpeta  
mkdir -p ruta/carpeta // Crea estructura completa  
cp /dev/null archivo.txt // Alternativa para crear archivo  

// 🔹 COPIAR
cp archivo.txt copia.txt // Copia archivo  
cp archivo.txt /ruta/ // Copia a otra ubicación  
cp -r carpeta/ copia/ // Copia carpeta completa  
cp -v archivo.txt copia.txt // Muestra proceso  
cp -u archivo.txt copia.txt // Solo si es más nuevo  

// 🔹 MOVER / RENOMBRAR
mv archivo.txt nuevo.txt // Renombra archivo  
mv archivo.txt /ruta/ // Mueve archivo  
mv carpeta/ /ruta/ // Mueve carpeta  
mv -v archivo.txt /ruta/ // Muestra proceso  

// 🔹 BORRAR
rm archivo.txt // Borra archivo  
rm -rf !(archivo.txt) // Borra todo menos archivo.txt (requiere extglob)
rm -f archivo.txt // Fuerza borrado sin preguntar  
rm -r carpeta/ // Borra carpeta recursiva  
rm -rf carpeta/ // Borra todo sin confirmación (PELIGRO)  
rmdir carpeta/ // Borra carpeta vacía  

// 🔹 BUSCAR
find /ruta -name archivo.txt // Busca por nombre  
find . -type f // Solo archivos  
find . -type d // Solo directorios  
find . -size +10M // Archivos mayores a 10MB  

// 🔹 ENLACES
ln archivo.txt enlace // Enlace duro  
ln -s archivo.txt enlace // Enlace simbólico  

// 🔹 INFORMACIÓN
file archivo.txt // Tipo de archivo  
stat archivo.txt // Info detallada  
du -sh carpeta/ // Tamaño total carpeta  
df -h // Espacio en disco  

// 🔹 COMPRESIÓN
tar -cvf archivo.tar carpeta/ // Crear tar  
tar -xvf archivo.tar // Extraer tar  
tar -czvf archivo.tar.gz carpeta/ // Comprimir gzip  
tar -xzvf archivo.tar.gz // Extraer gzip  

// 🔹 LIMPIEZA / UTILIDAD
clear // Limpia pantalla  
history // Historial comandos  
alias ll='ls -lh' // Alias útil  

// 🔹 RUTAS
pwd // Muestra ruta actual  
cd /ruta/ // Ir a ruta  
cd .. // Subir nivel  
cd ~ // Ir al home  

// 🔹 CONTROL AVANZADO (útil en laboratorio)

watch ls -l // Ejecuta comando en tiempo real  
xargs rm < lista.txt // Borra múltiples desde lista  
find . -type f -name "*.log" -delete // Borra logs  
find . -type f -exec rm {} \; // Borra con ejecución  
```

- `rm -rf` → destruye todo, sin vuelta atrás    
- `mv` → mover = también renombrar    
- `cp -r` → obligatorio para carpetas    
- `find` + `-delete` → limpieza masiva    
- `chmod +x` → ejecutables    
- `tail -f` → monitoreo en tiempo real (logs, seguridad)
