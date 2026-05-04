## Formatos

| Formato           | Extensión típica                                             | Usado por / compatibilidad            | Comentario                                                                        |
| ----------------- | ------------------------------------------------------------ | ------------------------------------- | --------------------------------------------------------------------------------- |
| **PPK**           | `.ppk`                                                       | PuTTY, WinSCP                         | Formato propietario de PuTTY; no se puede usar directamente en OpenSSH.           |
| **PEM**           | `.pem`                                                       | OpenSSH, OpenSSL, servidores web      | Base64 con encabezados `-----BEGIN PRIVATE KEY-----`; muy usado en TLS/SSL y SSH. |
| **OpenSSH**       | `id_rsa`, `id_ed25519` (sin extensión o `.pub` para pública) | OpenSSH, Linux/macOS, Windows OpenSSH | Formato nativo de OpenSSH. Claves privadas y públicas separadas.                  |
| **PKCS#8**        | `.key`, `.pem`                                               | OpenSSL, Java, algunos servicios      | Puede contener claves privadas en estándar interoperable; admite cifrado.         |
| **PKCS#12 / PFX** | `.p12`, `.pfx`                                               | Windows, OpenSSL                      | Contiene certificado + clave privada + cadena completa, usado para TLS/SSL.       |
| **CER / CRT**     | `.cer`, `.crt`                                               | Windows, Linux                        | Certificados públicos; no contienen la clave privada.                             |
| **DER**           | `.der`                                                       | Java, OpenSSL                         | Formato binario de certificado; alternativa a PEM.                                |

| Caso                             | Formato requerido            | ¿Qué contiene?                                                                                                         | ¿Por qué el aplicativo lo exige?                                                                                       | Ejemplos típicos                       |
| -------------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| **Certificados separados**       | **PEM (.pem / .crt / .key)** | - Clave privada (archivo aparte) - Certificado público - Cadena completa (full chain)                                  | El aplicativo quiere **control explícito** de cada componente y su ubicación. Facilita rotación, permisos y debugging. | Nginx, Apache, HAProxy, OpenVPN        |
| **Contenedor único**             | **PKCS#12 (.p12 / .pfx)**    | - Clave privada - Certificado público - Certificados intermedios _(todo en un solo archivo, protegido con contraseña)_ | Simplifica la importación y reduce errores humanos. El aplicativo solo sabe **importar un bundle**.                    | IIS, Tomcat, Java apps, Windows Server |
| **Entornos Java**                | **PKCS#12 / JKS**            | Igual que PKCS#12, almacenado en un keystore                                                                           | Java trabaja con **keystores**, no con archivos sueltos. Necesita un contenedor.                                       | Spring, WildFly, WebLogic              |
| **Dispositivos o appliances**    | **PKCS#12**                  | Bundle completo con password                                                                                           | Interfaces web limitadas; esperan **un archivo único** para carga manual.                                              | Firewalls, balanceadores, appliances   |
| **Automatización / DevOps**      | **PEM**                      | Archivos separados                                                                                                     | Facilita automatización, versionado y permisos granulares.                                                             | Kubernetes, Docker, CI/CD              |
| **Autenticación cliente (mTLS)** | **PKCS#12 o PEM**            | Clave + cert cliente                                                                                                   | Depende del stack: browsers y SO prefieren PKCS#12; servidores prefieren PEM.                                          | Browsers, VPNs, APIs                   |


| Formato           | Extensión              | Estándar         | Contenido                   | Codificación                  | Uso Común                     |
| ----------------- | ---------------------- | ---------------- | --------------------------- | ----------------------------- | ----------------------------- |
| Certificado X.509 | .cer / .crt            | X.509            | Clave pública + ID          | DER (Binario) o PEM (Base64)  | Sitios Web (HTTPS)            |
| Clave Privada SSH | (Ninguna) (ej. id_rsa) | OpenSSH / PKCS#1 | Clave privada               | PEM / Base64 (Texto)          | Autenticación local SSH       |
| Clave Pública SSH | .pub                   | RFC 4253         | Clave pública               | Base64 (una sola línea)       | Servidores (authorized_keys)  |
| PEM Key           | .key / .pem            | PKCS#8           | Clave privada               | PEM (Base64)                  | Servidores web (Nginx/Apache) |
| PKCS#12           | .pfx / .p12            | PKCS #12         | Cert + clave privada        | Binario (protegido con clave) | Exportar certificados Windows |
| PKCS#7            | .p7b                   | PKCS #7          | Solo cadena de certificados | PEM (Base64)                  | Instalación de certificados   |
|                   |                        |                  |                             |                               |                               |

| Algoritmo  | Seguridad  | Compatibilidad                | Recomendación             |
| ---------- | ---------- | ----------------------------- | ------------------------- |
| Ed25519    | Alta       | Muy buena (sistemas modernos) | Mejor opción hoy          |
| RSA (4096) | Alta       | Excelente (universal)         | Opción segura de respaldo |
| ECDSA      | Media/Alta | Buena                         | Solo si Ed25519 falla     |
| DSA        | Baja       | Mala (desactivado)            | No usar                   |


```sh
# 🔐 VARIABLES (MODIFICA AQUÍ)
 =========================
ALGO=rsa                    # rsa | ed25519 | ecdsa
BITS=2048                   # tamaño (solo RSA/ECDSA)
NAME=clave                  # nombre base
PASS="123456"               # passphrase
DAYS=365                    # días del certificado
SUBJ="/C=DO/ST=SD/L=SD/O=Lab/OU=IT/CN=example.com"

PRIV=$NAME.pem              # clave privada PEM
PUB=$NAME.pub               # clave pública
CERT=$NAME.crt              # certificado
PFX=$NAME.pfx               # contenedor PFX
DER=$NAME.der               # certificado DER
PPK=$NAME.ppk               # clave PuTTY



# 🔐 1. CREAR CLAVE (OpenSSH)
ssh-keygen -t $ALGO -b $BITS -f $NAME -N "$PASS" -C "lab@test"
# salida:
# $NAME        → privada (OpenSSH)
# $NAME.pub    → pública


# 🔄 2. CONVERTIR OpenSSH → PEM
ssh-keygen -p -m PEM -f $NAME -N "$PASS"


# 🔐 3. CREAR CLAVE PEM (OpenSSL)
openssl genpkey -algorithm RSA -out $PRIV -aes-256-cbc -pass pass:$PASS


# 🔐 4. PKCS#8 (formato estándar moderno)
openssl pkcs8 -topk8 -in $PRIV -out ${NAME}_pkcs8.pem \
-v2 aes-256-cbc -passin pass:$PASS -passout pass:$PASS


# 🔐 5. CREAR CERTIFICADO (CRT)
openssl req -x509 -newkey rsa:$BITS \
-keyout $PRIV -out $CERT -days $DAYS \
-subj "$SUBJ" -passout pass:$PASS


# 🔐 6. CREAR PFX / P12 (todo en uno)
openssl pkcs12 -export \
-out $PFX \
-inkey $PRIV \
-in $CERT \
-passin pass:$PASS \
-passout pass:$PASS

# 🔐 7. CONVERTIR PEM → DER
openssl x509 -in $CERT -outform der -out $DER

# 🔐 8. CONVERTIR A PPK (PuTTY)
puttygen $PRIV -o $PPK -O private -P

# 🔍 9. VER INFORMACIÓN
file $PRIV
openssl rsa -in $PRIV -text -noout -passin pass:$PASS
openssl x509 -in $CERT -text -noout


# 🔄 CONVERSIONES RÁPIDAS
 =========================
# OpenSSH → PEM
ssh-keygen -p -m PEM -f $NAME
# PEM → PPK
puttygen $PRIV -o $PPK
# PEM → PFX
openssl pkcs12 -export -out $PFX -inkey $PRIV -in $CERT
openssl pkcs12 -export -out $PFX -inkey $PEM -in $PEM
# PEM → DER
openssl x509 -in $CERT -outform der -out $DER



# 🔓 CRACKING (SI TIENE PASS)
ssh2john $NAME > hash.txt
john --wordlist=rockyou.txt hash.txt



# 🔥 RESUMEN
 =========================
# OpenSSH  → SSH (id_rsa, id_ed25519)
# PEM      → formato universal (texto base64)
# PKCS#8   → estándar moderno (privadas)
# PPK      → PuTTY
# PFX/P12  → clave + certificado + CA
# CRT/CER  → certificado público
# DER      → binario
```

