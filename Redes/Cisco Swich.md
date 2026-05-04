


# CONFIGURAR SNMP CISCO 
```bash
configure terminal                          # entrar a configuracion

snmp-server community public ro         # comunidad solo lectura
snmp-server community private rw            # (opcional) lectura/escritura

snmp-server host 172.40.60.4 version 2c ARS_Futuro   # servidor monitoreo
snmp-server enable traps                    # habilitar traps

snmp-server location "SITE-MDF"             # (opcional) ubicacion
snmp-server contact "admin@empresa.com"     # (opcional) contacto

end                                        # salir
write memory                               # guardar configuracion

# ----------VERIFICACION------------
show snmp community                        # ver comunidades
show snmp host                             # ver destinos
show snmp                                  # estado SNMP

# ----------PRUEBA DESDE LINUX------------
nmap -sU -p 161 172.40.60.4              # validar puerto UDP 161
snmpwalk -v2c -c public 172.40.60.4      # prueba SNMP real
```

# CORREGIR SNMP ACCESS (CISCO)------------
```bash
configure terminal                                   # entrar a config

# ----------QUITAR RESTRICCION ACTUAL------------
no snmp-server community public ro 172.40.60.4        # elimina restriccion por IP

# ----------CREAR ACL CORRECTA------------
access-list 10 permit 172.40.60.4                     # permite servidor NMS
access-list 10 permit 172.40.60.4                     # (opcional) otro NMS

# ----------APLICAR COMUNIDAD CON ACL------------
snmp-server community public ro 10                   # comunidad con ACL

# ----------CONFIGURAR HOST------------
snmp-server host 172.40.60.4 version 2c public       # destino traps

# ----------HABILITAR SNMP------------
snmp-server enable traps                             # habilitar traps
snmp-server server                                   # habilitar agente SNMP

end
write memory                                         # guardar

# ----------VERIFICACION------------
show snmp community                                  # validar ACL aplicada
show access-lists                                    # validar ACL
show snmp host                                       # validar destino

# ----------PRUEBA DESDE NMS------------
snmpwalk -v2c -c public 172.40.60.4                  # probar acceso SNMP
```