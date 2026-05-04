```sh
# --- DETECTAR BASE64

file archivo.txt                                  # Detecta si parece texto/base64 (no siempre exacto
base64 -d archivo.txt > /dev/null 2>&1 && echo "Es Base64 válido" || echo "No es Base64"   # Valida decodificando
grep -E '^[A-Za-z0-9+/=]+$' archivo.txt           # Verifica caracteres válidos Base64
strings archivo.txt | grep -E '^[A-Za-z0-9+/=]{20,}$'   # Busca cadenas largas tipo Base64
cat archivo.txt | tr -d '\n' | base64 -d          # Intenta decodificar quitando saltos de línea


# --- DETECTAR HEXADECIMAL

file archivo.bin                                  # Detecta si es binario/hex

grep -E '^[0-9a-fA-F]+$' archivo.txt              # Verifica si solo tiene caracteres hex

xxd archivo.bin | head                            # Ver contenido en hex

hexdump -C archivo.bin | head                     # Vista hex + ASCII

strings archivo.bin | grep -E '[0-9a-fA-F]{16,}'  # Busca cadenas largas hex


# --- DIFERENCIAR RÁPIDO

echo "48656c6c6f" | xxd -r -p                     # Hex → texto ("Hello")

echo "SGVsbG8=" | base64 -d                       # Base64 → texto ("Hello")


# --- DETECCIÓN AUTOMÁTICA (INTENTO)

cat archivo.txt | base64 -d > /dev/null 2>&1 && echo "Base64 válido"   # Si no falla → es base64
cat archivo.txt | xxd -r -p > /dev/null 2>&1 && echo "Hex válido"      # Si no falla → es hex

# --- 1) PRUEBA: ¿ES BASE64?
base64 -d archivo.txt > out.bin 2>/dev/null && echo "Base64 válido"

# --- 2) PRUEBA: ¿ES HEX?
xxd -r -p archivo.txt > out.bin 2>/dev/null && echo "Hex válido"


















# --- SEQ (la más simple)
seq -w 00000 99999 > diccionario.txt          # Genera 00000 → 99999
# --- BRUTE FORCE con loop bash
for i in {00000..99999}; do echo $i; done > diccionario.txt   # Genera todas las combinaciones

# --- PRINTF (más controlado)
for i in $(seq 0 99999); do printf "%05d\n" $i; done > diccionario.txt   # Rellena con ceros

# --- CRUNCH (herramienta profesional)
crunch 5 5 0123456789 > diccionario.txt       # Genera todas las combinaciones de 5 dígitos

# --- CRUNCH con patrón (solo números, fijo)
crunch 5 5 -t %%%%% > diccionario.txt         # % = número

# --- PYTHON (portable)
python3 -c "for i in range(100000): print(f'{i:05d}')" > diccionario.txt   # Genera 00000 → 99999

# --- AWK
awk 'BEGIN{for(i=0;i<=99999;i++) printf "%05d\n", i}' > diccionario.txt   # Genera combinaciones

# --- PERL
perl -e 'for($i=0;$i<=99999;$i++){printf "%05d\n",$i}' > diccionario.txt  # Genera combinaciones


```






```

while read pass; do
  openssl enc -aes-256-cbc -d -iter 100000 -in Imagen.enc -out img.zip -pass pass:$pass 2>/dev/null && echo "PASS: $pass
" && break
done < biblioteca.txt


while read pass; do   openssl enc -aes-256-cbc -d -iter 100000 -in Imagen.enc -out img.jpeg.gpg -pass pass:$pass 2>/dev/null && echo "PASS: $pass" && break; done < biblioteca.txt


while read pass; do
  openssl enc -aes-256-cbc -d -iter 100000 -in Imagen.enc -out img.jpeg -pass pass:$pass 2>/dev/null && echo "PASS: $pass" && break
done < biblioteca.txt



```



```
#!/bin/bash

INPUT="Imagen.enc"
WORDLIST="diccionario.txt"

ALGOS=(
  "aes-256-cbc"
)

KDFS=(
  ""
  "-pbkdf2 -iter 100000"
)

for algo in "${ALGOS[@]}"; do
  for kdf in "${KDFS[@]}"; do

    echo "[*] Probando: $algo $kdf"

    while read -r pass; do

      rm -f tmp.out

      openssl enc -d -$algo $kdf -in "$INPUT" -out tmp.out -pass pass:"$pass" 2>/dev/null

      # validar que el archivo no esté vacío
      if [[ ! -s tmp.out ]]; then
        continue
      fi

      TYPE=$(file -b tmp.out)

      # --- VALIDACIÓN REAL (firmas conocidas)
      if grep -aq "PK" tmp.out || \
         grep -aq "JFIF" tmp.out || \
         grep -aq "PNG" tmp.out || \
         grep -aq "PDF" tmp.out || \
         [[ "$TYPE" != "data" ]]; then

        echo "🔥 PASSWORD ENCONTRADA: $pass"
        echo "✔ Algoritmo: $algo $kdf"
        echo "✔ Tipo detectado: $TYPE"

        mv tmp.out resultado.bin
        exit 0
      fi

    done < "$WORDLIST"

  done
done

echo "❌ No se encontró contraseña"

```



```
#!/bin/bash

INPUT="Imagen.enc"

check_magic() {
  MAGIC=$(xxd -p -l 4 "$1")
  case "$MAGIC" in
    504b0304) echo "ZIP"; return 0 ;;
    ffd8ffe0|ffd8ffe1) echo "JPG"; return 0 ;;
    89504e47) echo "PNG"; return 0 ;;
    25504446) echo "PDF"; return 0 ;;
    *) return 1 ;;
  esac
}

ALGOS=(
  aes-256-cbc
)

KDFS=(
  ""
  "-pbkdf2"
  "-pbkdf2 -iter 100000"
)

for algo in "${ALGOS[@]}"; do
  for kdf in "${KDFS[@]}"; do

    echo "[*] $algo $kdf"

    for i in $(seq -w 00000 99999); do

      openssl enc -d -$algo $kdf \
        -in "$INPUT" -out tmp \
        -pass pass:$i 2>/dev/null

      [[ ! -s tmp ]] && continue

      check_magic tmp && echo "🔥 PASS: $i | $algo $kdf" && exit

    done

  done
done

echo "❌ No encontrado"
```



```
# Probar contrase;a

for opt in "" "-pbkdf2 -iter 100000"; do
  openssl enc -aes-256-cbc -d $opt -in Imagen.enc -out salida -pass pass:09777 2>/dev/null
  echo "[*] Probando $opt"
  file salida
done


for algo in aes-128-cbc aes-192-cbc aes-256-cbc; do
  openssl enc -d -$algo -in Imagen.enc -out salida -pass pass:09777 2>/dev/null
  echo "[*] $algo"
  file salida
done
```