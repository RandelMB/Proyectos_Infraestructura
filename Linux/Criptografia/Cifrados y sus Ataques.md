```

```
# 🔐 1. CIFRADO SIMÉTRICO (misma clave)
## 🔹 Algoritmos
- RSA
- ECC (más moderno)
- DSA (firma, no cifrado directo)
## 🔹 Herramientas típicas
```bash
openssl enc -aes-256-cbc -in file -out file.enc
gpg -c file
7z a archivo.7z file
zip -e archivo.zip file
```
## 🔴 Ataque openssl
```bash
# openssl (manual)
while read pass; do
  openssl enc -aes-256-cbc -d -iter 100000 -in Imagen.enc -out out -pass pass:$pass 2>/dev/null && echo "PASS: $pass" && break
done < diccionario.txt

while read pass; do
  openssl enc -aes-256-cbc -d -in Imagen.enc -out Imagen.jpg -pass pass:$pass 2>/dev/null && echo "Contraseña encontrada: $pass" && break
done < rockyou.txt

```

```sh
openssl enc -aes-256-cbc -salt -in archivo.txt -out archivo.enc   # cifrar archivo con OpenSSL (AES-256)

openssl enc -d -aes-256-cbc -in archivo.enc -out salida           # descifrar archivo OpenSSL con password manual

openssl enc -d -aes-256-cbc -md md5 -in archivo.enc -out salida   # descifrar usando derivación MD5 (default antiguo)

openssl enc -d -aes-256-cbc -md sha256 -in archivo.enc -out salida # descifrar usando SHA256 en la derivación de clave

while read pass; do openssl enc -d -aes-256-cbc -in archivo.enc -out out -pass pass:$pass 2>/dev/null && echo "PASS: $pass" && break; done < diccionario.txt   # fuerza bruta básica openssl

# -------------ATAQUE A SECUENCIA DE 5 DIGISTOS NUMERICOS-------------- 
for i in $(seq -w 00000 99999); do
  openssl enc -d -aes-256-cbc -in secreto_recuperado.txt -out test.png -pass pass:$i 2>/dev/null
  file test.png | grep -q PNG && echo "PASSWORD: $i" && break
done


openssl2john Imagen.enc > hash.txt       # convertir .enc a hash para John (solo útil si usa MD5)
john --wordlist=rockyou.txt hash.txt      # ataque de diccionario con John
john --show hash.txt                      # mostrar contraseñas encontradas

hashcat -m 18600 hash.txt rockyou.txt     # ataque con Hashcat para OpenSSL


gpg -c archivo.txt                        # cifrar archivo con GPG (simétrico)
gpg -d archivo.gpg                        # descifrar archivo GPG
gpg2john archivo.gpg > hash.txt           # convertir GPG a hash para cracking

zip -e archivo.zip archivo.txt            # crear ZIP protegido con contraseña
fcrackzip -u -D -p rockyou.txt archivo.zip # crackear ZIP directamente
zip2john archivo.zip > hash.txt           # convertir ZIP a hash

7z a archivo.7z archivo.txt               # crear archivo 7z cifrado
7z x archivo.7z                           # extraer archivo 7z (pedirá password)
7z2john archivo.7z > hash.txt             # convertir 7z a hash

rar a -p archivo.rar archivo.txt          # crear RAR con contraseña
rar x archivo.rar                         # extraer RAR
rar2john archivo.rar > hash.txt           # convertir RAR a hash

pdf2john archivo.pdf > hash.txt           # extraer hash de PDF protegido
office2john archivo.docx > hash.txt       # extraer hash de documentos Office

ssh2john id_rsa > hash.txt                # extraer hash de clave privada SSH

steghide embed -cf imagen.jpg -ef secreto.txt   # ocultar archivo en imagen
steghide extract -sf imagen.jpg                # extraer datos ocultos
stegseek imagen.jpg rockyou.txt               # ataque de diccionario a esteganografía


```

###### file -b archivo : detectar tipo general del archivo (rápido, pero poco fiable solo)

| Salida de `file`          | Tipo real          | Interpretación            |
| ------------------------- | ------------------ | ------------------------- |
| JPEG image data           | Imagen JPG         | ✔ Imagen válida           |
| PNG image data            | Imagen PNG         | ✔ Imagen válida           |
| GIF image data            | Imagen GIF         | ✔ Imagen válida           |
| Zip archive data          | ZIP                | ✔ Archivo comprimido      |
| gzip compressed data      | GZIP               | ✔ Comprimido válido       |
| PDF document              | PDF                | ✔ Documento válido        |
| ASCII text                | Texto plano        | ✔ Archivo legible         |
| UTF-8 Unicode text        | Texto válido       | ✔ Contenido correcto      |
| Microsoft Word 2007+      | DOCX               | ✔ Documento (ZIP interno) |
| ELF 64-bit LSB executable | Binario Linux      | ✔ Ejecutable válido       |
| PE32 executable           | Ejecutable Windows | ✔ Binario válido          |
| MP3 audio                 | Audio              | ✔ Archivo multimedia      |
| MP4 ISO Base Media        | Video              | ✔ Archivo multimedia      |


```c
strings archivo | head  
xxd archivo | head
```

| Tipo de archivo           | `strings` (patrón visible) | Magic bytes (`xxd -p -l 4`) | Resultado                 |
| ------------------------- | -------------------------- | --------------------------- | ------------------------- |
| 🔒 OpenSSL cifrado        | Salted__                   | 53616c74                    | ✔ Cifrado (no descifrado) |
| ❌ Basura (mal descifrado) | `=oWB`, `d+m5`, `3{/>A`    | Variable (sin patrón)       | ❌ Incorrecto              |
| 🖼 PNG                    | PNG, IHDR, IDAT            | 89504e47                    | ✔ Imagen válida           |
| 🖼 JPG                    | JFIF, Exif                 | ffd8ff                      | ✔ Imagen válida           |
| 🖼 GIF                    | GIF8                       | 47494638                    | ✔ Imagen válida           |
| 📄 PDF                    | %PDF                       | 25504446                    | ✔ Documento válido        |
| 📦 ZIP                    | PK                         | 504b0304                    | ✔ Archivo comprimido      |
| 📦 RAR                    | Rar!                       | 52617221                    | ✔ Archivo comprimido      |
| 📦 GZIP                   | gzip                       | 1f8b08                      | ✔ Comprimido              |
| 🎵 MP3                    | ID3                        | 494433                      | ✔ Audio válido            |
| 🎬 MP4                    | ftyp                       | 00000018                    | ✔ Video                   |
| 🐧 ELF                    | ELF                        | 7f454c46                    | ✔ Binario Linux           |
| 🪟 EXE                    | MZ                         | 4d5a                        | ✔ Ejecutable Windows      |
| 📁 TAR                    | ustar                      | 75737461                    | ✔ Archivo TAR             |
| 📦 7Z                     | 7z                         | 377abcaf                    | ✔ Comprimido              |


![[Pasted image 20260329085638.png]]





```sh
#!/bin/bash

DIR="${1:-.}"

echo "=== SCAN DE PATRONES VALIDOS ==="

for file in "$DIR"/*; do
  [ -f "$file" ] || continue

  # Obtener magic bytes (8 bytes por seguridad)
  MAGIC=$(xxd -p -l 8 "$file" 2>/dev/null)

  TYPE=""
  EXTRA=""

  # === DETECCION POR MAGIC BYTES (FUENTE DE VERDAD) ===
  case "$MAGIC" in
    89504e47*) TYPE="PNG" ;;
    ffd8ff*) TYPE="JPG" ;;
    47494638*) TYPE="GIF" ;;
    25504446*) TYPE="PDF" ;;
    504b0304*) TYPE="ZIP/DOCX/APK" ;;
    52617221*) TYPE="RAR" ;;
    1f8b08*) TYPE="GZIP" ;;
    377abcaf*) TYPE="7Z" ;;
    424d*) TYPE="BMP" ;;
    494433*) TYPE="MP3" ;;
    00000018*) TYPE="MP4" ;;
    7f454c46*) TYPE="ELF" ;;
    4d5a*) TYPE="EXE" ;;
    7573746172*) TYPE="TAR" ;;
    *) TYPE="" ;;
  esac

  # === FILTRO: SOLO ARCHIVOS CON FIRMA VALIDA ===
  if [[ -n "$TYPE" ]]; then

    # Validaciones adicionales (strings útiles)
    STR=$(strings "$file" 2>/dev/null | grep -E -m 3 "JFIF|Exif|PNG|IHDR|IDAT|PDF|PK|XML")

    # file para confirmación
    FILETYPE=$(file -b "$file")

    echo "-----------------------------"
    echo "FILE: $file"
    echo "TYPE: $TYPE"
    echo "MAGIC: $MAGIC"
    echo "FILE CMD: $FILETYPE"

    if [[ -n "$STR" ]]; then
      echo "STRINGS:"
      echo "$STR"
    fi

    # binwalk solo si tiene sentido
    echo "BINWALK:"
    binwalk "$file" 2>/dev/null | head -n 3

    # exiftool solo si es imagen
    if [[ "$TYPE" == "PNG" || "$TYPE" == "JPG" || "$TYPE" == "GIF" ]]; then
      echo "EXIF:"
      exiftool "$file" 2>/dev/null | grep -E "File Type|Image Width|Image Height" | head -n 3
    fi

  fi

done

echo "=== FIN ==="
```


