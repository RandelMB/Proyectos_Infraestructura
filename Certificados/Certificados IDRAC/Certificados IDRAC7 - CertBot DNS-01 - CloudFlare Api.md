
# Mediante Letsencript con cerbot en Linux

## Lo primero es Crear API Token en Cloudflare


![[20260227114855.png]](https://github.com/RandelMB/Proyectos_Infraestructura/blob/main/img/Pasted%20image%2020260227114855.png)



#### Cuando le des a crear y editar zona DNS, elije estos Permisos:  Zone → DNS → Edit , y despues elige tu dominio



![[Pasted image 20260227115027.png]](https://github.com/RandelMB/Proyectos_Infraestructura/blob/main/img/Pasted%20image%2020260227115027.png)



![[Pasted image 20260227115341.png]](https://github.com/RandelMB/Proyectos_Infraestructura/blob/main/img/Pasted%20image%2020260227115341.png)


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
### en mi caso yo utilicé el protocolo SFTP (bitvirse) para transferirlo a Windows también puedes usara Wincsp o filezilla

![[Pasted image 20260227123645.png]](https://github.com/RandelMB/Proyectos_Infraestructura/blob/main/img/Pasted%20image%2020260227123645.png)


#### Carac

![[Pasted image 20260227123925.png]](https://github.com/RandelMB/Proyectos_Infraestructura/blob/main/img/Pasted%20image%20260227123925.png)


