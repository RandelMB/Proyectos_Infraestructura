
# Acceso Seguro mediante HTTPs a Ntopng (Cloudflare y letsencrypt)
####  Crear API Token en Cloudflare
En Cloudflare:
1. **My Profile → API Tokens    
2. **Create Token**    
3. Template: **Edit zone DNS**    
4. Permisos mínimos:    
    - `Zone:DNS:Edit`        
    - `Zone:Zone:Read`        
5. Limitar al dominio (ejemplo `midominio.com`)    

### Crear archivo de credenciales (En le host donde se corre docker) 
```bash
# Crea carpeta
mkdir -p ~/letsencrypt
nano ~/letsencrypt/cloudflare.ini

# Dentro pon tu api
dns_cloudflare_api_token = TU_API_TOKEN

# Lectura y escritura solo a root
chmod 600 ~/letsencrypt/cloudflare.ini
```
### Obtener el certificado con DNS-01 (De forma manual)
```bash
# Instalar Certbot
apt install certbot python3-certbot-dns-cloudflare

# Solicitar Cretificado
certbot certonly \
--dns-cloudflare \
--dns-cloudflare-credentials ~/letsencrypt/cloudflare.ini \
-d ntop.midominio.com

# se creara un registro TXT
_acme-challenge.ntop.midominio.com

# Valida que se genero el certificado
certbot certificates
/etc/letsencrypt/live/ntop.midominio.com/          #ubicacion

# se van a generar esos 2 archivos (Ntop los requiere unidos)
fullchain.pem
privkey.pem

# Agrega el Volumen a la carpeta de letsencrypt
volumes:
  - /etc/letsencrypt:/certs:ro
```

###  Montar certificado en contenedor Ntopng
```yaml
# Ntopng requiere los certificados unidos
cat fullchain.pem privkey.pem > ntopng-cert.pem

# Acceder al Contenedor de Ntopng
docker exec -it ntopng bash

# Copiar Certificado al directorio requeido
cp /certs/live/ntop.cybersupports.org/ntopng-cert.pem /usr/share/ntopng/httpdocs/ssl/ntopng-cert.pem
```

#### Se visualiza certificado correctamente

![[Pasted image 20260310162714.png]]
###  Renovación automática
```bash
# Certbot instala un cron automático
certbot renew --dry-run
```

### Conclusion
Para la próxima vez utilizo traefik