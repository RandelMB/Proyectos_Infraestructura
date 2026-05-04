

```sh
cp archivo.cer /usr/local/share/ca-certificates/firewall.crt     # Copiar el certificado
update-ca-certificates                                           # Actualizar 
```

### Para que funcione con docker
```sh
mkdir -p /etc/docker/certs.d/registry-1.docker.io            # Se crea una carpeta en docker
cp /usr/local/share/ca-certificates/firewall.crt \
/etc/docker/certs.d/registry-1.docker.io/ca.crt              # Se copia el certificado anterior a donde va
```
