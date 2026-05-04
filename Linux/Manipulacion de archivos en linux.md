```bash
# VISUALIZACION DE LOGS (TAIL)
tail -n 50 archivo.log                 # últimas 50 líneas
tail -f archivo.log                    # seguimiento en tiempo real
tail -F archivo.log                    # seguimiento con rotación
tail -n +100 archivo.log               # desde línea 100
tail -f archivo1.log archivo2.log      # múltiples logs
tail -v -f *.log                       # logs con encabezado
tail -f -s 2 archivo.log               # intervalo 2s
tail -c 500 archivo.log                # últimos 500 bytes
tail -f archivo.log | grep ERROR       # filtrar errores en vivo
less +F archivo.log                    # alternativa interactiva

# BUSQUEDA (GREP)
grep "texto" archivo.txt               # buscar coincidencias
grep -i "error" archivo.txt            # ignorar mayúsculas
grep -n "error" archivo.txt            # mostrar líneas
grep -E "error|fail" archivo.txt       # múltiples patrones
grep -v "error" archivo.txt            # excluir patrón
grep -w "root" archivo.txt             # palabra exacta
grep -c "error" archivo.txt            # contar coincidencias
grep -A 3 "error" archivo.txt          # 3 líneas después
grep -B 3 "error" archivo.txt          # 3 líneas antes
grep -r "texto" /ruta/                 # búsqueda recursiva

# EDICION (SED)
sed 's/old/new/g' archivo.txt          # reemplazar texto
sed '/pattern/d' archivo.txt           # eliminar líneas
sed -n '/pattern/p' archivo.txt        # filtrar líneas
sed -i 's/old/new/g' archivo.txt       # editar en sitio

# PROCESAMIENTO (AWK/CUT/SORT)
awk '/error/ {print}' archivo.txt      # filtrar líneas
awk '{print $1}' archivo.txt           # primera columna
cut -d":" -f1 archivo.txt              # extraer campo
sort archivo.txt                       # ordenar
sort archivo.txt | uniq                # eliminar duplicados

# VISTA RAPIDA
cat archivo.txt                        # mostrar completo
less archivo.txt                       # paginado
head -n 50 archivo.txt                 # primeras 50 líneas
wc -l archivo.txt                      # contar líneas

# MANIPULACION ARCHIVOS
ls -lah                                # listar detallado
touch archivo.txt                      # crear archivo
mkdir -p ruta/carpeta                  # crear estructura
cp -r origen/ destino/                 # copiar carpeta
mv archivo.txt nuevo.txt               # renombrar
rm -rf carpeta/                        # eliminar carpeta
find . -name "*.log"                   # buscar archivos
du -sh *                               # tamaños
df -h                                  # espacio disco

# COMPRESION
tar -czvf archivo.tar.gz carpeta/      # comprimir
tar -xzvf archivo.tar.gz               # extraer

# ECHO Y REDIRECCION
echo "texto" > archivo.txt             # sobrescribir
echo "texto" >> archivo.txt            # append
echo -e "linea1\nlinea2"               # saltos línea
echo "$VAR"                            # variable
echo "texto" | base64                  # codificar
echo "dGV4dG8=" | base64 -d            # decodificar

# VERIFICACION
ls -lh                                 # validar archivos
cat archivo.txt | head                 # validar contenido
```

# SCP
```bash
# SCP DIRECTORIO
scp -r /ruta/origen usuario@host:/ruta/destino/        # copiar carpeta completa

# SCP ARCHIVO
scp /ruta/origen/archivo.ext usuario@host:/ruta/destino/   # copiar archivo

# SCP CON PUERTO
scp -r -P PUERTO /ruta/origen usuario@host:/ruta/destino/  # puerto personalizado

# SCP DESCARGAR
scp -r usuario@host:/ruta/remota /ruta/local              # descargar carpeta

# SCP VERBOSE
scp -rv /ruta/origen usuario@host:/ruta/destino/          # modo debug

# VERIFICACION
ls -l /ruta/origen                         # validar origen
ssh usuario@host "ls -l /ruta/destino/"    # validar destino
```