
## Opciones/Parámetros clave de Openssl .enc :
```sh
// 🔐 ALGORITMOS (cifrado real que se usa)
-aes-256-cbc      # AES 256 bits modo CBC (fuerte, estándar)
-aes-192-cbc      # AES 192 bits modo CBC
-aes-128-cbc      # AES 128 bits modo CBC (más rápido)
-aes-256-ctr      # AES modo CTR (tipo stream, sin padding)
-aes-128-ctr      # AES CTR más rápido
-aes-256-cfb      # AES modo CFB (stream, no padding)
-aes-256-ofb      # AES modo OFB (stream continuo)
-des-cbc          # DES (obsoleto, inseguro)
-des3             # Triple DES (viejo, evitar)
-rc4              # Stream cipher antiguo (inseguro)
-chacha20         # Cipher moderno rápido (si está disponible)

// 📂 ENTRADA / SALIDA (manejo de archivos)
-in archivo.txt   # archivo de entrada (datos originales)
-out archivo.enc  # archivo de salida (cifrado o descifrado)

// 🔑 CONTRASEÑA (cómo se define la clave)
-k "clave123"     # contraseña directa (rápido, menos seguro en scripts)
-pass pass:clave  # contraseña explícita (recomendado en scripts)
-pass file:clave.txt # lee contraseña desde archivo
-pass env:VAR     # usa variable de entorno (más seguro en automatización)

// 🔄 MODO (qué operación ejecutar)
-e                # cifrar (encrypt, por defecto)
-d                # descifrar (decrypt)

// 🧂 DERIVACIÓN DE CLAVE (CRÍTICO PARA SEGURIDAD)
-salt             # añade 8 bytes aleatorios al proceso de derivación (evita ataques precomputados)
-nosalt           # sin salt (inseguro)
-pbkdf2           # para convertir password → clave (recomendado, derivación moderna)
-iter 100000      # número de iteraciones PBKDF2 (más = más seguro/lento)
-md sha256        # hash usado para derivar clave (mejor que md5)

// 🔤 FORMATO / CODIFICACIÓN (salida del archivo)
-a                # salida en base64 (texto ASCII)
-base64           # igual que -a
-A                # base64 en una sola línea (sin saltos)

// 🔍 DEBUG / INFORMACIÓN (para análisis)
-p                # muestra clave e IV usados (debug)
-P                # muestra clave/IV y NO ejecuta
-v                # modo verbose (más detalles en ejecución)

// ⚙️ CONTROL / PARÁMETROS AVANZADOS
-K 001122...      # clave en hexadecimal (sin usar password)
-iv 001122...     # vector de inicialización manual
-bufsize 8192     # tamaño del buffer de lectura/escritura
-nopad            # desactiva padding (requiere bloques exactos)
-z                # comprime antes de cifrar (poco usado)
-engine nombre    # usa motor criptográfico externo (hardware, etc.)

// 📦 EXTRA (útiles en laboratorio / auditoría)
-nosalt           # hace el cifrado más fácil de crackear (para pruebas)
-pbkdf2 -iter 1   # derivación débil (simular malas prácticas)
```


```sh
// 🔐 CIFRADO / DESCIFRADO (enc)
openssl enc -aes-256-cbc -salt -in archivo.txt -out archivo.enc          # Cifrar básico AES-256
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -in archivo.txt -out archivo.enc  # Cifrado fuerte recomendado

openssl enc -aes-256-cbc -d -in archivo.enc -out archivo.txt             # Descifrar básico
openssl enc -aes-256-cbc -d -pbkdf2 -iter 100000 -in archivo.enc -out archivo.txt     # Descifrar con PBKDF2

openssl enc -aes-256-ctr -in archivo.txt -out archivo.enc                # Cifrado modo CTR (sin padding)
openssl enc -chacha20 -in archivo.txt -out archivo.enc                   # Cifrado moderno rápido

openssl enc -aes-256-cbc -a -in archivo.txt -out archivo.enc             # Cifrado en base64 (texto)
openssl enc -aes-256-cbc -d -a -in archivo.enc -out archivo.txt          # Descifrar base64

openssl enc -aes-256-cbc -in archivo.txt -out archivo.enc -k "clave"     # Password directa
openssl enc -aes-256-cbc -in archivo.txt -out archivo.enc -pass pass:clave  # Password explícita

openssl enc -aes-256-cbc -nosalt -in archivo.txt -out archivo.enc        # Sin salt (débil, laboratorio)


// 🔓 ATAQUE DICCIONARIO (fuerza bruta con OpenSSL)
while read pass; do openssl enc -aes-256-cbc -d -in archivo.enc -out salida.txt -pass pass:$pass 2>/dev/null && echo "CLAVE: $pass" && break; done < rockyou.txt  # Ataque diccionario


// 🔍 HASHING (integridad / cracking)
openssl md5 archivo.txt        # Hash MD5 (inseguro)
openssl sha1 archivo.txt       # Hash SHA1 (débil)
openssl sha256 archivo.txt     # Hash SHA256 (recomendado)
openssl sha512 archivo.txt     # Hash SHA512 (más fuerte)
openssl dgst -sha256 archivo.txt   # Forma alternativa SHA256
openssl dgst -md5 archivo.txt      # Hash MD5
openssl dgst -sha256 -hmac "clave" archivo.txt  # HMAC con clave


// 🔑 GENERACIÓN DE CLAVES
openssl rand -base64 16        # Genera clave aleatoria base64
openssl rand -hex 16           # Genera clave en hexadecimal

openssl genrsa -out privada.pem 2048   # Genera clave RSA privada
openssl rsa -in privada.pem -pubout -out publica.pem   # Extrae clave pública

openssl ecparam -genkey -name prime256v1 -out privada.pem # Genera clave ECC prime256v1
openssl ec -in privada.pem -pubout -out publica.pem # Saca la pública

openssl genpkey -algorithm ed25519 -out privada.pem # Clave privada ed25519
openssl pkey -in privada.pem -pubout -out publica.pem # Pública

openssl genpkey -algorithm x25519 -out privada.pem # Clave privada x25519
openssl pkey -in privada.pem -pubout -out publica.pem # Pública


// 🔐 CIFRADO ASIMÉTRICO (RSA)
openssl rsautl -encrypt -inkey publica.pem -pubin -in archivo.txt -out archivo.enc   # Cifrar con clave pública
openssl rsautl -decrypt -inkey privada.pem -in archivo.enc -out archivo.txt          # Descifrar con clave privada


// 📜 CERTIFICADOS (SSL/TLS)
openssl req -new -x509 -key privada.pem -out cert.pem -days 365   # Crear certificado autofirmado
openssl x509 -in cert.pem -text -noout   # Ver contenido del certificado
openssl s_client -connect google.com:443   # Ver handshake TLS
openssl s_client -connect host:443 -showcerts   # Ver certificados del servidor


// 🔎 ANÁLISIS / INSPECCIÓN
openssl version               # Versión de OpenSSL
openssl list -cipher-algorithms   # Lista algoritmos disponibles
openssl list -digest-algorithms   # Lista funciones hash
openssl enc -ciphers          # Lista cifrados soportados
openssl asn1parse -in archivo.pem   # Analiza estructura ASN.1


// 📦 CONVERSIÓN DE FORMATOS
openssl base64 -in archivo.txt -out archivo.b64   # Codificar base64
openssl base64 -d -in archivo.b64 -out archivo.txt   # Decodificar base64
openssl rsa -in privada.pem -text -noout   # Ver detalles de clave privada
openssl pkcs12 -export -out archivo.p12 -inkey privada.pem -in cert.pem   # Crear PKCS#12


// ⚙️ DEBUG / PRUEBAS
openssl enc -aes-256-cbc -p -in archivo.txt -out archivo.enc   # Muestra clave e IV
openssl enc -aes-256-cbc -P -in archivo.txt                    # Solo muestra clave/IV sin ejecutar
openssl speed aes-256-cbc   # Benchmark de rendimiento
```