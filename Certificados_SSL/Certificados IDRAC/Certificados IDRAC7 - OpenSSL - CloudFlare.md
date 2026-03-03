
## Generar Certificados en Windows

```
# Instala Open SSL
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

### En la opccion de ssl/tls le das la sollicitud CSR a cloudflare o a cualquier CAD que tengas

![[Pasted image 20260227103031.png]]


```
# Es algo como esto

-----BEGIN CERTIFICATE REQUEST-----  
MIIC...  
-----END CERTIFICATE REQUEST-----


Simplemente creas un archivo en formato .pem
```

**Cloudflare** solo me entrega el certificado en formatos:
- PEM    
- PKCS#7    
- DER    

Pero **iDRAC7** necesita **PKCS#12 (.pfx)**, por lo que tenemos que convertirlo.

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

##### En caso de que utilices cloudflare  te aparecerá de la siguiente manera por lo que debes descargar la CA raíz de Cloudflare para instalarla en el navegador


![[Pasted image 20260227105916.png]]

![[Pasted image 20260227110325.png]]

##### Si en en Firefox → Ajustes → Privacidad y Seguridad → Certificados → Ver certificados → Autoridades → Importar


![[Pasted image 20260227110240.png]]



