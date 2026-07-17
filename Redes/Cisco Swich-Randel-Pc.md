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

# CORREGIR SNMP ACCESS (CISCO)
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

# HABILITAR SNMPV3
```bash
configure terminal                                 # Entrar en configuración global
snmp-server server                                 # Habilitar servicio SNMP
snmp-server view ALL iso included                  # Crear vista completa
snmp-server group NMS-GRP v3 priv read ALL         # Crear grupo SNMPv3 con auth+priv
snmp-server engineid remote 172.40.5.8 800000090300A1B2C3D4E5 # Registrar EngineID remoto manualmente  
snmp-server user snmpv3 NMS-GRP v3 auth sha256 'ClaveAuth123' priv aes 'ClavePriv123' remote 172.40.5.8 # Crear usuario remoto SNMPv3

snmp-server host 172.x.x.x version 3p priv snmpv3 # Configurar destino traps SNMPv3
snmp-server enable traps                           # Habilitar traps SNMP
snmp-server trap authentication                    # Habilitar trap por fallo autenticación
snmp-server contact NOC                            # Contacto SNMP
snmp-server location IDF-F                         # Ubicación SNMP

end                                                # Salir
write memory                                       # Guardar configuración
```

# VERIFICAR CONFIGURACION
```bash
show snmp                                          # Ver estado general SNMP
show running-config | include snmp                # Ver configuración SNMP activa

show snmp user                                     # Ver usuarios SNMPv3
show snmp group                                    # Ver grupos SNMPv3
show snmp view                                     # Ver vistas SNMP
show snmp host                                     # Ver destinos traps
show snmp engineid                                 # Ver EngineID SNMPv3
```

# PRUEBA DESDE LINUX

```bash
snmpwalk -v3 -u snmpv3 -l authPriv -a SHA -A 'MiClaveAuth123' -x AES -X 'MiClavePriv123' 172.25.X.X 1.3.6.1.2.1.1   # Test SNMPv3
```

# OPCION SHA256 SI EL SWITCH LO SOPORTA

```bash
snmp-server user snmpv3 NMS-GRP auth sha-256 MiClaveAuth123 priv aes 128 MiClavePriv123   # Usuario con SHA256
```

# ELIMINAR CONFIGURACION SNMPV3
```bash
configure terminal                                # Entrar configuración

no snmp-server user snmpv3                        # Eliminar usuario

no snmp-server group NMS-GRP v3                  # Eliminar grupo

no snmp-server view ALL                           # Eliminar vista

no snmp-server host 172.25.9.28                  # Eliminar host traps

end                                               # Salir

write memory                                      # Guardar
```