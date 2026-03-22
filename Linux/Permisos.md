
![[Pasted image 20260322095413.png|699]]
# Permisos y Propietarios
```sh
# Ver permisos
ls -l
stat archivo

# Conf de Dueños
chown usuario archivo.txt          # cambia el dueño del archivo
chown usuario:grupo archivo.txt    # cambia dueño y grupo
chown :grupo archivo.txt           # cambia solo el grupo
chown -R usuario /opt/ips-monitor  # cambia dueño recursivamente
chown -R usuario:grupo /opt/ips-monitor  # cambia dueño y grupo en todo el directorio
chown root archivo.txt             # asigna como dueño a root
chown root:root archivo.txt        # dueño y grupo root
chown usuario1:grupo1 archivo.txt  # asigna dueño y grupo específicos
chown --from=usuario1 usuario2 archivo.txt  # cambia dueño solo si actualmente es usuario1
chown -R usuario:grupo /var/www    # típico para servidores web
chown -R www-data:www-data /var/www  # a usuario del servidor web
chown -h usuario enlace_simbolico  # cambia dueño del symlink (no del archivo real)
chown -R --preserve-root /         # evita afectar la raíz completa (seguridad)
```

```sh
chmod -R 750 /opt/ips-monitor    # dueño: rwx, grupo: r-x, otros: --- 
chmod -R 755 /opt/ips-monitor    # dueño: rwx, grupo: r-x, otros: r-x (lectura/ejecución pública)
chmod -R 700 /opt/ips-monitor    # solo el dueño tiene acceso total (privado)
chmod -R 770 /opt/ips-monitor    # dueño y grupo: rwx, otros: --- (trabajo en equipo cerrado)
chmod -R 777 /opt/ips-monitor    # todos: rwx (⚠️ inseguro, evitar en producción)
chmod 644 script.py             # dueño: rw-, grupo: r--, otros: r-- 
chmod 600 script.py             # dueño: rw-, grupo: ---, otros: ---
chmod 755 script.py             # archivo ejecutable: dueño rwx, otros r-x
chmod +x script.py              # añade permiso de ejecución a todos
chmod u+x script.py             # añade ejecución solo al dueño
chmod g+x script.py             # añade ejecución al grupo
chmod o+x script.py             # añade ejecución a otros
chmod u-r script.py             # quita permiso de lectura al dueño
chmod g-w script.py             # quita escritura al grupo
chmod o-rwx script.py           # quita todos los permisos a otros
chmod 640 config.conf           # dueño: rw-, grupo: r--, otros: --- (config compartida con grupo)
chmod 444 archivo.txt           # solo lectura para todos
chmod 000 archivo.txt           # nadie puede leer, escribir ni ejecutar
chmod -R u+rwX,g+rX,o-rwx /opt/ips-monitor  
# dueño: lectura/escritura (+ejecución si ya era directorio o ejecutable), grupo: lectura/ejecución, otros: sin acceso
```