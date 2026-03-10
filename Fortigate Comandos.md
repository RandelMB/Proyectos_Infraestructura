



## Rutas

```
# Ver Rutas
get router info routing-table all

# Validar por donde estamos saliendo
get router info routing-table details 10.0.11.1



config router static
    edit 2
        set dst 10.0.11.0 255.255.255.0
        set device "port5"
        set gateway 190.190.190.2
    next
end

config router static
    edit 2
        set dst 192.168.0.0 255.255.255.0
        set device "port5"
        set gateway 190.190.190.1
    next
end
```