
```csharp
// ======================= 🔐 CIFRADO =======================
gpg --symmetric --cipher-algo AES256 secreto.txt        // Cifra archivo con AES256 usando contraseña

steghide embed -cf USA-TRUMP.jpg -ef secreto.txt -sf USA-TRUMP-PAZ.jpg   // Oculta archivo dentro de imagen
steghide embed -cf nba.jpg -ef secretimagen.txt -sf nbasecreto.jpg       // Inserta texto oculto en imagen

base64 archivo.txt                                     // Codifica contenido
base64 archivo.txt > archivo.b64 // Guarda resultado

echo "texto" | base64                                  // Codifica texto en Base64
echo "texto" | tr 'A-Za-z' 'N-ZA-Mn-za-m'              // Aplica ROT13 (ofuscación básica)
echo -n "P@ssw0rd" | base64                           // Codifica sin salto de línea

xxd -p archivo.txt // Convierte a hex  
xxd -p archivo.txt > archivo.hex // Guarda en hex

openssl enc -aes-256-cbc -salt -in archivo.txt -out archivo.enc //  ENCRIPTAR a .enc (AES-256)
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -in archivo.txt -out archivo.enc // Cifra más seguro

// ======================= 🔓 DESCIFRADO =======================
gpg --output secreto_recuperado.txt --decrypt secreto.txt.gpg   // Descifra archivo GPG
gpg --output secreto_recuperado1.txt --decrypt secreto_recuperado.txt.gpg   // Descifra otro archivo

echo "VGV4dG8=" | base64 -d                         // Decodifica Base64
echo "Guvf vf zl frperg zrffntr" | tr 'A-Za-z' 'N-ZA-Mn-za-m'   // Decodifica ROT13

xxd -r -p archivo.hex > archivo.txt // Decodificar de HEX

steghide extract -sf corrupta.jpeg                 // Extrae datos ocultos de imagen
steghide extract -sf imagen.jpg                    // Extrae contenido oculto
steghide extract -sf nbasecreto.jpg                // Recupera archivo escondido

stegseek mlb.jpg rockyou.txt                       // Rompe steghide rápido

openssl enc -aes-256-cbc -d -in archivo.enc -out archivo.txt // Descifra   
openssl enc -aes-256-cbc -d -pbkdf2 -iter 100000 -in Imagen.enc -out Imagen.jpg // Descifra (si usaste pbkdf2)  

// Bucle wile para ataque de fuerza bruta 
while read pass; do
  openssl enc -aes-256-cbc -d -in Imagen.enc -out Imagen.jpg -pass pass:$pass 2>/dev/null && echo "Contraseña encontrada: $pass" && break
done < rockyou.txt



//------------------------------- CREAR BIBLIOTECA ----------------------------------
crunch 8 8 abc123 -o biblioteca.txt                 // combinaciones de 8 chars (limitado)
crunch 6 6 "1234" -o biblioteca.txt                 // combinaciones de 8 chars (limitado)
crunch 6 8 0123456789 -o numeros.txt               // números de 6 a 8 dígitos
crunch 6 8 abc123 -o simple.txt                    // letras + números básicos
tr -dc '1-9' < /dev/urandom | fold -w 6 | head -n 1000 > claves.txt // 1000 claves aleatorias de 6 dígitos


// =======================  ANÁLISIS =======================

cat rockyou.txt custom.txt > mega.txt              // unir 2 bibliotecas

// Identificar archivos
file corrupta.jpeg                                // Detecta tipo real de archivo
file -i vpnuser.key                               // Muestra tipo MIME
file *                                            // Analiza todos los archivos

// Buscar archivos
find . -type f                                    // Lista todos los archivos
find . -type f -name "*.key"                      // Busca archivos .key
find . -type f -size +10M                         // Busca archivos grandes

// Ver contenido parcial
head vpnuser.key                                  // Primeras líneas
head -20 vpnuser.key                              // Primeras 20 líneas
tail vpnuser.key                                  // Últimas líneas
tail -20 vpnuser.key                              // Últimas 20 líneas
tail -f vpnuser.key                               // Monitorea en tiempo real

// Extraer texto
strings vpnuser.key                               // Extrae texto legible
strings -n 10 vpnuser.key                         // Texto mínimo de 10 caracteres
strings -t x vpnuser.key                          // Muestra offsets en hex
strings vpnuser.key | grep password               // Busca "password"
strings vpnuser.key | grep user                   // Busca "user"

// Hexadecimal
xxd vpnuser.key                                   // Ver en hex
xxd -l 200 vpnuser.key                            // Primeros 200 bytes
xxd -r archivo.hex > archivo.bin                  // Reconstruye binario
hexdump -C vpnuser.key | less                     // Vista detallada hex
od -c vpnuser.key                                 // Vista en caracteres
od -x vpnuser.key                                 // Vista en hex

// Información
stat vpnuser.key                                  // Metadata completa
ls -lh vpnuser.key                                // Tamaño legible
ls -la vpnuser.key                                // Permisos y ocultos

// Búsqueda
grep "password" vpnuser.key                       // Busca texto
grep -i "user" vpnuser.key                        // Ignora mayúsculas
grep -r "password" .                              // Busca recursivo

// Integridad
md5sum archivo                                    // Hash MD5
sha1sum archivo                                   // Hash SHA1
sha256sum archivo                                 // Hash SHA256
sha512sum archivo                                 // Hash SHA512

// Entropía
ent vpnuser.key                                   // Mide aleatoriedad (detecta cifrado)

// METADATA avanzada
exiftool vpnuser.key                              // Metadata
exiftool -a vpnuser.key                           // Todo duplicado
exiftool -u vpnuser.key                           // Campos ocultos
exiftool corrupta.zip                             // Intenta leer metadatos en ZIP  
zipinfo corrupta.zip                              // Muestra estructura interna del ZIP
exiftool -Comment="UEBzc3cwcmQK" nba.jpg          // Inserta comentario codificado  
exiftool --info nba.jpg                           // Muestra info extendida  
exiftool nba.jpg | grep Comment                   // Filtra comentario

// Imágenes
identify -verbose corrupta.jpeg                   // Info completa de imagen

// Análisis binario
binwalk vpnuser.key                               // Detecta archivos embebidos
binwalk -e vpnuser.key                            // Extrae contenido
binwalk --dd='.*' vpnuser.key                     // Extrae todo

// Hash cracking
hashid corrupta.jpeg                              // Identifica tipo de hash
gpg2john txt.txt.gpg > hash.txt                   // Convierte GPG a hash
john --wordlist=rockyou.txt hash.txt              // Ataque diccionario

// Comparación
diff archivo1 archivo2                            // Compara archivos

// VISUALIZACIÓN / IMÁGENES  
display nba // Abre imagen  
display nba.jpg // Abre imagen específica

// 🔹 MAT2 (limpieza forense)  
mat2 nba.jpg // Limpia metadatos  
mat2 -s nba.jpg // Modo silencioso  
mat2 -v nba.jpg // Modo verbose  
mat2 -L nba.jpg // Lista formatos soportados

// =======================  ESTEGANOGRAFÍA =======================

// Análisis
zsteg -a corrupta.jpeg                            // Detecta datos ocultos en imagen
steghide info corrupta.jpeg                       // Info sobre datos ocultos

// Ataques
stegseek imagen.jpg                               // Fuerza bruta contraseña
stegseek imagen.jpg rockyou.txt                   // Usa diccionario
stegseek corrupta.jpeg /usr/share/wordlists/rockyou.txt   // Ataque con wordlist

// Recuperación
foremost vpnuser.key                              // Recupera archivos
foremost -i vpnuser.key                           // Especifica input

// Descargas útiles
wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt   // Descarga diccionario

// ======================= 📦 CIFRADO ZIP (AVANZADO / ALTERNATIVAS) =======================

zip -er backup_seguro.zip carpeta/                // Crea ZIP cifrado (AES en versiones modernas)
zip -e secreto2.zip secreto2.txt                  // Crear ZIP con contraseña
zip -9 -e datos.zip archivo.txt                   // Máxima compresión + cifrado

7z a -tzip -p archivo.zip secreto.txt             // Crear ZIP usando 7zip con contraseña
7z a -tzip -p -mem=AES256 seguro.zip carpeta/     // ZIP con cifrado AES256 (más fuerte que zip clásico)

7z l archivo.zip                                 // Lista contenido del ZIP
7z l -slt archivo.zip                            // Lista con detalles técnicos (incluye cifrado)

unzip -l archivo.zip                             // Lista contenido sin extraer
unzip -t archivo.zip                             // Verifica integridad del ZIP

zip -sf archivo.zip                              // Muestra archivos dentro del ZIP
zip -T archivo.zip                               // Testea si el ZIP está corrupto

zip -FF corrupto.zip --out reparado.zip          // Intenta reparar ZIP dañado
zip -F parcialmente_corrupto.zip --out fix.zip   // Reparación básica

dd if=archivo.zip of=parte.zip bs=1 count=1000000   // Extrae parte del ZIP (forense/recovery)

cat archivo.zip | strings | grep password        // Busca posibles contraseñas dentro del ZIP
binwalk archivo.zip                              // Detecta datos ocultos dentro del ZIP

zipgrep password archivo.zip                     // Busca texto dentro de archivos comprimidos

7z t seguro.zip                                  // Verifica integridad en 7z
7z h archivo.zip                                 // Calcula hash del ZIP

zipdetails archivo.zip                           // Muestra estructura interna del ZIP (forense)





// ======================= 🔓 FCRACKZIP (LABORATORIO DE AUDITORÍA) =======================  

Formato            Herramienta        Modo Hashcat (-m)   # Descripción
ZIP (Legacy)       zip2john           17200               # ZIP clásico (PKZIP), más débil
WinZip (AES)       zip2john           13600               # ZIP moderno con AES (más seguro)
RAR5               rar2john           13000               # RAR versión 5 (fuerte)
7-Zip              7z2john.pl         11600               # 7z (AES-256, bastante robusto)

// 🔹 Información básica    
fcrackzip -v archivo.zip // Muestra información del ZIP  
fcrackzip -h // Ayuda del comando  
  
// 🔹 Ataque de diccionario (PRIMERO SIEMPRE)    
fcrackzip -u -D -p rockyou.txt archivo.zip // Diccionario + valida contraseña  
fcrackzip -D -p rockyou.txt archivo.zip // Diccionario sin validación (más rápido)  
fcrackzip -u -D -p rockyou.txt -v archivo.zip // Diccionario con progreso  
fcrackzip -u -D -p claves.txt archivo.zip // Usa diccionario personalizado  
fcrackzip -u -D -p rockyou.txt -l 6-12 archivo.zip // Filtra por longitud  
  
// 🔹 Ataque de fuerza bruta (si falla diccionario)   
fcrackzip -b archivo.zip // Fuerza bruta básica  
fcrackzip -b -v archivo.zip // Fuerza bruta con progreso  
  
fcrackzip -b -c a archivo.zip // Solo letras minúsculas  
fcrackzip -b -c A archivo.zip // Solo letras mayúsculas  
fcrackzip -b -c 1 archivo.zip // Solo números  
fcrackzip -b -c a1 archivo.zip // Minúsculas + números  
fcrackzip -b -c aA1 archivo.zip // Minúsculas + mayúsculas + números

// 🔹 Control de longitud y rango (más preciso)
fcrackzip -b -l 4 archivo.zip // Fuerza bruta solo contraseñas de longitud 4  
fcrackzip -b -l 4-8 archivo.zip // Fuerza bruta entre 4 y 8 caracteres  

// 🔹 Definir charset personalizado
fcrackzip -b -c abc123 archivo.zip // Usa solo los caracteres indicados  
fcrackzip -b -c aA1! archivo.zip // Incluye símbolos personalizados  

// 🔹 Optimización y rendimiento
fcrackzip -b -u -c a1 -l 6-8 archivo.zip // Fuerza bruta optimizada validando contraseña  
fcrackzip -D -u -p rockyou.txt -v -l 8 archivo.zip // Diccionario solo de longitud exacta 8  

// 🔹 Modo silencioso / scripting
fcrackzip -D -p rockyou.txt -q archivo.zip // Modo silencioso (para scripts)  
fcrackzip -b -q archivo.zip // Fuerza bruta sin salida en pantalla  

// 🔹 Selección de archivo dentro del ZIP
fcrackzip -b -u -c a1 archivo.zip -f archivo.txt // Ataca usando un archivo específico dentro del zip  

// 🔹 Límite de intentos (control de ejecución)
fcrackzip -b -c a1 -l 1-6 -t 100000 archivo.zip // Limita cantidad de intentos  

// 🔹 Multiprocesamiento (si está soportado)
fcrackzip -b -c a1 -l 6-8 -u -v -P archivo.zip // Usa múltiples procesos (depende del build)

// 🔹 Mostrar solo la contraseña encontrada
fcrackzip -b -c a1 -l 6-8 -u -q archivo.zip // Solo imprime la clave, útil para automatización  

// 🔹 Continuar ataque (reanudación manual aproximada)
fcrackzip -b -c a1 -l 6-8 archivo.zip > output.txt // Guarda progreso para análisis externo  

// 🔹 Combinación avanzada (realista en auditoría)
fcrackzip -u -D -p rockyou.txt -l 8-12 -v archivo.zip // Diccionario con filtro realista de longitud  






//----------- GENERAR HASH DESDE ARCHIVOS----------------------------

zip2john archivo.zip > hash.txt // Extrae hash de ZIP  
zip2john -o archivo.txt archivo.zip > hash.txt // Extrae hash de un solo archivo dentro del ZIP  
rar2john archivo.rar > hash.txt // Extrae hash de RAR  
7z2john.pl archivo.7z > hash.txt // Extrae hash de 7z  

// 🔹 LIMPIAR HASH (OBLIGATORIO PARA HASHCAT)

cat hash.txt // Ver contenido del hash  
cut -d ':' -f2 hash.txt > clean.txt // Quita nombre del archivo (deja solo el hash)  
head -n 1 hash.txt > hash1.txt // Usar solo primera línea (evitar multi-file)  

// 🔹 IDENTIFICAR TIPO DE ARCHIVO

file archivo.zip // Detecta tipo de archivo  
zipinfo -v archivo.zip // Ver detalles (tipo de cifrado, método, etc.)  

// 🔹 HASHCAT (CRACKING)

hashcat -m 17200 clean.txt rockyou.txt // ZIP clásico (PKZIP)  
hashcat -m 13600 clean.txt rockyou.txt // ZIP AES (WinZip)  
hashcat -m 13000 clean.txt rockyou.txt // RAR5  
hashcat -m 11600 clean.txt rockyou.txt // 7z  

// 🔹 HASHCAT CON REGLAS

hashcat -m 17200 clean.txt rockyou.txt -r rules/best64.rule // Usa reglas de mutación  

// 🔹 VER RESULTADOS

hashcat -m 17200 clean.txt rockyou.txt --show // Muestra contraseñas encontradas  

// 🔹 JOHN THE RIPPER (ALTERNATIVA)

john hash.txt --wordlist=rockyou.txt // Crackea con John  
john --show hash.txt // Muestra resultado crackeado  








// ======================= EXIV2 (metadatos de imágenes)=======================

// Básicos
exiv2 archivo.jpg // Muestra metadatos básicos (EXIF/IPTC/XMP)  
exiv2 -p a archivo.jpg // Muestra TODOS los metadatos (all)  
exiv2 -p e archivo.jpg // Solo EXIF  
exiv2 -p i archivo.jpg // Solo IPTC  
exiv2 -p x archivo.jpg // Solo XMP  

// Detallado y estructurado
exiv2 -pa archivo.jpg // Metadatos con estructura legible  
exiv2 -pv archivo.jpg // Metadatos en formato detallado (verbose)  

// Extraer información específica
exiv2 -g Date archivo.jpg // Filtra por etiquetas que contengan "Date"  
exiv2 -g GPS archivo.jpg // Filtra datos GPS  
exiv2 -g Camera archivo.jpg // Filtra info de cámara  

// Exportar / extraer
exiv2 -e a archivo.jpg // Extrae todos los metadatos a archivos separados  
exiv2 -e e archivo.jpg // Extrae solo EXIF  
exiv2 -e x archivo.jpg // Extrae solo XMP  

// Modificar metadatos
exiv2 -M"set Exif.Image.Make Canon" archivo.jpg // Cambia fabricante de cámara  
exiv2 -M"set Exif.Image.Model HackerCam" archivo.jpg // Cambia modelo  
exiv2 -M"del Exif.Image.Make" archivo.jpg // Elimina campo  

// Copiar metadatos
exiv2 -t origen.jpg destino.jpg // Copia metadatos de una imagen a otra  

// Borrar metadatos
exiv2 rm archivo.jpg // Elimina TODOS los metadatos  
exiv2 -d a archivo.jpg // Borra EXIF/IPTC/XMP  

// Renombrar por fecha
exiv2 rename archivo.jpg // Renombra según fecha EXIF  

// 🔹 IDENTIFY (ImageMagick)

// Básicos
identify archivo.jpg // Muestra info básica (resolución, formato, tamaño)  
identify -verbose archivo.jpg // Información completa de la imagen  

// Formato personalizado
identify -format "%f %wx%h" archivo.jpg // Nombre + resolución  
identify -format "%m %wx%h %b" archivo.jpg // Formato + tamaño + peso  

// Metadatos específicos
identify -format "%[EXIF:DateTime]" archivo.jpg // Fecha EXIF  
identify -format "%[EXIF:Make]" archivo.jpg // Marca de cámara  
identify -format "%[EXIF:Model]" archivo.jpg // Modelo  

// Información de color
identify -format "%k colors" archivo.jpg // Número de colores  
identify -format "%r" archivo.jpg // Espacio de color  

// Lotes (varios archivos)
identify *.jpg // Analiza todas las imágenes JPG  
identify -format "%f %wx%h\n" *.png // Lista resolución de varios archivos  

// Integración útil en auditoría
identify -verbose archivo.jpg | grep -i exif // Filtra metadatos EXIF  
identify -verbose archivo.jpg | grep -i profile // Busca perfiles incrustados  
```



# OpenSSL para cifrar, descifrar y analizar
```bash
// 🔹 CIFRADO / DESCIFRADO SIMÉTRICO (archivos)

// AES (recomendado)
openssl enc -aes-256-cbc -salt -in archivo.txt -out archivo.enc // Cifra con AES-256  
openssl enc -aes-256-cbc -d -in archivo.enc -out archivo.txt // Descifra  

openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -in archivo.txt -out archivo.enc // Cifra con derivación fuerte (mejor práctica)  
openssl enc -aes-256-cbc -d -pbkdf2 -iter 100000 -in archivo.enc -out archivo.txt // Descifra con mismos parámetros  

// Base64 (codificación, no cifrado)
openssl enc -base64 -in archivo.txt -out archivo.b64 // Codifica  
openssl enc -base64 -d -in archivo.b64 -out archivo.txt // Decodifica  

// Otros algoritmos
openssl enc -aes-128-cbc -in archivo.txt -out archivo.enc // AES-128  
openssl enc -des-ede3 -in archivo.txt -out archivo.enc // 3DES (obsoleto)  

// 🔹 HASHES (integridad)

openssl dgst -sha256 archivo.txt // Hash SHA-256  
openssl dgst -md5 archivo.txt // MD5 (no seguro)  
openssl dgst -sha1 archivo.txt // SHA1 (débil)  

openssl dgst -sha256 -verify pubkey.pem -signature firma.sig archivo.txt // Verifica firma  

// 🔹 CLAVES PRIVADAS / PÚBLICAS (RSA)

// Generar clave privada
openssl genpkey -algorithm RSA -out privada.pem -pkeyopt rsa_keygen_bits:2048 // Genera clave privada  

// Extraer clave pública
openssl rsa -in privada.pem -pubout -out publica.pem // Saca clave pública  

// Cifrar con clave pública
openssl rsautl -encrypt -pubin -inkey publica.pem -in archivo.txt -out archivo.enc // Cifra  

// Descifrar con clave privada
openssl rsautl -decrypt -inkey privada.pem -in archivo.enc -out archivo.txt // Descifra  

// 🔹 FIRMAS DIGITALES

openssl dgst -sha256 -sign privada.pem -out firma.sig archivo.txt // Firma archivo  
openssl dgst -sha256 -verify publica.pem -signature firma.sig archivo.txt // Verifica firma  

// 🔹 CERTIFICADOS (X.509)

// Ver certificado
openssl x509 -in cert.pem -text -noout // Muestra detalles  

openssl x509 -in cert.pem -noout -dates // Fechas de validez  
openssl x509 -in cert.pem -noout -issuer // Emisor  
openssl x509 -in cert.pem -noout -subject // Sujeto  

// Convertir formatos
openssl x509 -in cert.pem -outform DER -out cert.der // PEM → DER  
openssl x509 -in cert.der -inform DER -out cert.pem // DER → PEM  

// 🔹 CSR (solicitud de certificado)

openssl req -new -key privada.pem -out solicitud.csr // Crear CSR  

// 🔹 CONEXIONES SSL/TLS (ANÁLISIS)

// Probar conexión TLS
openssl s_client -connect google.com:443 // Test conexión TLS  

openssl s_client -connect google.com:443 -showcerts // Muestra toda la cadena  

// Forzar versión
openssl s_client -connect google.com:443 -tls1_2 // Fuerza TLS 1.2  
openssl s_client -connect google.com:443 -tls1_3 // Fuerza TLS 1.3  

// Ver cipher negociado
openssl s_client -connect google.com:443 | grep Cipher // Muestra cifrado usado  

// 🔹 LISTAR ALGORITMOS

openssl list -cipher-algorithms // Cifrados disponibles  
openssl list -digest-algorithms // Hash disponibles  

// 🔹 GENERAR DATOS ALEATORIOS

openssl rand -base64 32 // Genera clave aleatoria  
openssl rand -hex 16 // Genera bytes en hex  

// 🔹 PKCS12 (certificados con clave)

// Ver contenido .p12
openssl pkcs12 -in archivo.p12 -info // Muestra contenido  

// Extraer clave privada
openssl pkcs12 -in archivo.p12 -nocerts -out privada.pem // Extrae clave  

// Extraer certificado
openssl pkcs12 -in archivo.p12 -clcerts -nokeys -out cert.pem // Extrae cert  
```


