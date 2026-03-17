```python
### 1. Filtrar por una palabra

grep "palabra" archivo.txt

Muestra **solo las líneas que contienen esa palabra**.

Ejemplo:
# Filtrar palabra
grep "error" logs.txt

### 2. Ignorar mayúsculas y minúsculas
grep -i "error" logs.txt

### 3. Mostrar número de línea
grep -n "error" logs.txt

### 4. Filtrar varias palabras
grep -E "error|fail|warning" logs.txt

### 5. Excluir una palabra
grep -v "error" logs.txt

### 6. Buscar coincidencia exacta
grep -w "root" archivo.txt

### 7. Contar cuántas coincidencias hay
grep -c "error" logs.txt

### 9. Ver contexto (líneas antes o después)
grep -A 3 "error" logs.txt
grep -B 3 "error" logs.txt


### Comando que más se usa en servidores

grep -i "error" /var/log/syslog

Sirve para **buscar errores en logs del sistema**.

---

Si quieres, te puedo mostrar **5 combinaciones de `grep` que usan mucho los administradores Linux en entrevistas técnicas** (muy comunes en soporte y sysadmin).

grep es el único comando que hay o dame otras variantes, otros comandos distintos.

`grep` es el más usado, pero **no es el único**. En Linux hay varios comandos para **filtrar o buscar texto dentro de archivos**.

|Comando|Para qué sirve|Ejemplo|
|---|---|---|
|**grep**|Buscar líneas que contengan una palabra|`grep "error" archivo.txt`|
|**egrep**|Buscar usando expresiones regulares más avanzadas|`egrep "error|
|**fgrep**|Buscar texto literal (más rápido porque no usa regex)|`fgrep "192.168.1.1" archivo.txt`|
|**awk**|Filtrar texto por columnas o patrones|`awk '/error/ {print}' archivo.txt`|
|**sed**|Filtrar y modificar texto en flujo|`sed -n '/error/p' archivo.txt`|
|**cut**|Extraer columnas específicas|`cut -d":" -f1 /etc/passwd`|
|**sort**|Ordenar líneas de un archivo|`sort archivo.txt`|
|**uniq**|Eliminar líneas repetidas|`sort archivo.txt|
|**head**|Ver primeras líneas|`head -10 archivo.txt`|
|**tail**|Ver últimas líneas|`tail -10 archivo.txt`|


### Ejemplos prácticos de filtrado

**1. Buscar errores en logs**

grep error /var/log/syslog

**2. Filtrar por columna**

awk '{print $1}' archivo.txt

**3. Buscar palabra con sed**

sed -n '/error/p' archivo.txt

# tiempo real filtrando
tail -f /var/log/syslog | grep ssh

### Los 4 comandos más usados por administradores Linux

1. `grep` → buscar texto    
2. `awk` → procesar y filtrar datos    
3. `sed` → modificar o filtrar texto    
4. `cut` → extraer columnas
    

```