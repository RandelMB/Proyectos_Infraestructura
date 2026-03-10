

```python
netstat
netstat -a
netstat -b
netstat -e
netstat -f
netstat -n
netstat -o
netstat -p tcp
netstat -p udp
netstat -p ip
netstat -p icmp
netstat -p tcpv6
netstat -p udpv6
netstat -r
netstat -s
netstat -t
netstat -x
netstat -y
netstat -ab
netstat -an
netstat -ano
netstat -e -s
netstat -r -n
netstat -a -n -o
netstat -b -n -o
netstat -a -b
netstat -a -e
netstat -a -f
netstat -a -p tcp
netstat -a -p udp

# Muestra el nombre del aplicativo
netstat -abno

# Filtra por puerto
netstat -ano | findstr :80
netstat -ano | findstr :443
netstat -ano | findstr :22
netstat -ano | findstr :3389
netstat -ano | findstr :8080
netstat -ano | findstr ESTABLISHED  
netstat -ano | findstr LISTENING


```


```python
# VISUALIZACION BASICA DE CONEXIONES
# ===============================

# Muestra las conexiones TCP activas
netstat

# Muestra todas las conexiones y puertos en escucha
netstat -a

# Muestra direcciones y puertos en formato numerico (no resuelve DNS)
netstat -n

# Muestra todas las conexiones en formato numerico
netstat -an
```

```
# IDENTIFICACION DE PROCESOS
# ===============================

# Muestra el PID del proceso asociado a cada conexion
netstat -o

# Muestra conexiones, formato numerico y PID del proceso
netstat -ano

# Muestra el ejecutable que creo la conexion (requiere administrador)
netstat -b

# Muestra todos los puertos y el ejecutable responsable
netstat -ab

# Modo detallado (usado normalmente junto con -b)
netstat -vb

# Variante detallada con todas las conexiones y procesos
netstat -vab
```

```
# ESTADISTICAS DE RED
# ===============================

# Estadisticas de la interfaz de red (bytes enviados/recibidos)
netstat -e

# Estadisticas detalladas por protocolo
netstat -s

# Estadisticas solo del protocolo TCP
netstat -sp tcp

# Estadisticas solo del protocolo UDP
netstat -sp udp
```

```
# TABLA DE ENRUTAMIENTO
# ===============================

# Muestra la tabla de enrutamiento del sistema
netstat -r

# Variante equivalente
route print
```

```c
# MONITOREO EN TIEMPO REAL
//# Actualiza las conexiones cada 5 segundos
netstat -ano 5

//# Actualiza cada 2 segundos
netstat -ano 2

//# Actualizar cada 5 segundos + filtro
netstat -ano 5 | findstr :443
```

```
# FILTROS POR PUERTOS
# ===============================

# Filtra conexiones en el puerto 80
netstat -ano | findstr :80

# Filtra conexiones en el puerto 443
netstat -ano | findstr :443

# Filtra conexiones en el puerto 53 (DNS)
netstat -ano | findstr :53
```

```
# FILTROS POR ESTADO
# ===============================

# Muestra conexiones establecidas
netstat -ano | findstr ESTABLISHED

# Muestra puertos en escucha
netstat -ano | findstr LISTENING

# Muestra conexiones cerrandose
netstat -ano | findstr TIME_WAIT
```

```
# RESOLUCION DE NOMBRES
# ===============================

# Muestra el FQDN de las direcciones remotas
netstat -f

# Variante combinada con PID
netstat -f -o
```

```
# DIAGNOSTICO AVANZADO
# ===============================

# Muestra todos los puertos incluyendo TIME_WAIT
netstat -q

# Muestra plantilla de conexion TCP
netstat -y

# Variante completa para auditoria
netstat -abno
```