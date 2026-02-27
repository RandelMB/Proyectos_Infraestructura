---
tags:
---
#### HTTP-01 Challege - +
```python
# Abrir puerto 80 en el modem
# Crear registro que apunte a tu ip publica

# Validar Ip Publica
curl portquiz.net

#validar que este llegando a tu ip publica
nslookup adguard.ejemplo.net

#abrir puertos para recepcion
iptables -I INPUT -i eth0 -p tcp --dport 80 -j ACCEPT
iptables -D INPUT 1

# comando para solicitud de certificado
sudo certbot certonly --standalone -d adguard.ejemplo.net
```
### <font color="#9bbb59"> Archivos de certificado</font>
```
/etc/letsencrypt/live/ejemplo.net/fullchain.pem
/etc/letsencrypt/live/ejemplo.net/privkey.pem

Certificate is saved at: /etc/letsencrypt/live/adguard.ejemplo.net/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/adguard.ejemplo.net/privkey.pem
```
## Generar la clave SSH desde Window
```python
# --GENERADOR DE LLAVES (SELECCIONA 1)-- #
ssh-keygen -t rsa -b 4096 -C "ejemp@ejemplo.com"
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/id_ecdsa_521
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519

# --
type $env:USERPROFILE\.ssh\id_rsa.pub

# -- EN UBUNTU --#
chmod 700 ~/.ssh/authorized_keys

nano ~/.ssh/authorized_keys
sudo systemctl restart ssh
```

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

|Caso|Formato requerido|¿Qué contiene?|¿Por qué el aplicativo lo exige?|Ejemplos típicos|
|---|---|---|---|---|
|**Certificados separados**|**PEM (.pem / .crt / .key)**|- Clave privada (archivo aparte) - Certificado público - Cadena completa (full chain)|El aplicativo quiere **control explícito** de cada componente y su ubicación. Facilita rotación, permisos y debugging.|Nginx, Apache, HAProxy, OpenVPN|
|**Contenedor único**|**PKCS#12 (.p12 / .pfx)**|- Clave privada - Certificado público - Certificados intermedios _(todo en un solo archivo, protegido con contraseña)_|Simplifica la importación y reduce errores humanos. El aplicativo solo sabe **importar un bundle**.|IIS, Tomcat, Java apps, Windows Server|
|**Entornos Java**|**PKCS#12 / JKS**|Igual que PKCS#12, almacenado en un keystore|Java trabaja con **keystores**, no con archivos sueltos. Necesita un contenedor.|Spring, WildFly, WebLogic|
|**Dispositivos o appliances**|**PKCS#12**|Bundle completo con password|Interfaces web limitadas; esperan **un archivo único** para carga manual.|Firewalls, balanceadores, appliances|
|**Automatización / DevOps**|**PEM**|Archivos separados|Facilita automatización, versionado y permisos granulares.|Kubernetes, Docker, CI/CD|
|**Autenticación cliente (mTLS)**|**PKCS#12 o PEM**|Clave + cert cliente|Depende del stack: browsers y SO prefieren PKCS#12; servidores prefieren PEM.|Browsers, VPNs, APIs|
# Generar Certificados en Windows

##### Instala Open SSL con winget
```
    > winget install --id FireDaemon.OpenSSL
    > winget install --id ShiningLight.OpenSSL.Dev
    > winget install --id ShiningLight.OpenSSL.Light
```


```python
# Generar clave privada
openssl genrsa -out idrac.key 2048

# Generar CSR (Solicitud de Certificado)
openssl req -new -key idrac.key -out idrac.csr
```


```python
# convertir a PKCS#12 si esta en (.pem)
openssl pkcs12 -export -out idrac.pfx -inkey idrac.key -in idrac.pem

# Si esta en PKCS#7 (.p7b) 
openssl pkcs7 -print_certs -in certificado.p7b -out certificado.pem

# Si está en DER (.cer)
openssl x509 -inform der -in certificado.cer -out certificado.pem


# te pedira pass

Enter Export Password:
Verifying - Enter Export Password:
```

# Mediante Letsencript con cerbot en Linux

#### Lo primero es Crear API Token en Cloudflare o cualquier otro 

```python
# Crear archivo de credenciales - En un linux
nano cloudflare.ini

# con esto:
dns_cloudflare_api_token = TU_TOKEN_

# Proteger permisos:
chmod 600 cloudflare.ini

# Generar certificado DNS-01 automático
sudo certbot certonly \  
--dns-cloudflare \  
--dns-cloudflare-credentials cloudflare.ini \  
-d idrac.tudominio.org

# Creará automáticamente el registro TXT `_acme-challenge`
# tu certificado tiene que estar en:
/etc/letsencrypt/live/idrac.ejemplo.org/
```

### Puedes convertirlo desde le Linux o desde Windows

```python
# linux:
openssl pkcs12 -export \
-out idrac.pfx \
-inkey privkey.pem \
-in fullchain.pem

# Windows
openssl pkcs12 -export -out idrac.pfx -inkey privkey.key -in fullchain.pem

# te pedira pass
Enter Export Password:
Verifying - Enter Export Password:
```

