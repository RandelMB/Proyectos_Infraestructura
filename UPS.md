
**SNMP Trap Receivers** define a qué servidor se enviarán alertas y qué tipo de eventos se reportarán mediante **SNMP**.
## Tipos de TRAP

### RFC1628 Trap
Es el estándar definido para UPS en la MIB:
RFC 1628
Sirve para enviar eventos estándar como:
- cambio a batería    
- batería baja    
- retorno de energía    
- sobrecarga    
- fallo del UPS    

Características:
- formato **estándar SNMP UPS-MIB**    
- compatible con sistemas de monitoreo como    
    - Observium        
    - Zabbix        
    - PRTG Network Monitor        
En general **es el que se debe activar para monitoreo**.

### EPPC Trap

EPPC significa **Eaton Power Protocol / Proprietary Communication**.
Son traps **propietarios de Eaton** usados por software de Eaton como:
- Eaton Intelligent Power Manager    
- Eaton Intelligent Power Protector    

Características:
- incluyen eventos más específicos del fabricante    
- algunos NMS no los interpretan correctamente.    

## Severity

La opción **Severity** filtra qué nivel de eventos se enviarán.
### Information
Eventos informativos.
Ejemplos:
- test de batería    
- conexión de red    
- estado normal del UPS    
- cambios de configuración    
No indican problema.

### Warning
Eventos que requieren atención pero no son críticos.
Ejemplos:
- batería descargándose    
- temperatura alta    
- carga elevada    
- batería envejecida    

### Severe
Eventos críticos.
Ejemplos:
- fallo del UPS    
- batería muy baja    
- sobrecarga grave    
- apagado inminente    

## Configuración típica recomendada

Para monitoreo con Observium u otro NMS:
- **Trap type**    
    - RFC1628 → habilitado        
    - EPPC → opcional
        
- **Severity**    
    - Warning        
    - Severe


