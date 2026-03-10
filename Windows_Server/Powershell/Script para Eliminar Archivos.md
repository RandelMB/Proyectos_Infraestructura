


```powershell

# Ruta donde están los accesos directos
$Ruta = "C:\Ruta\Donde\Estan\Los\Archivos"

# Lista exacta de archivos a eliminar
$Archivos = @(
    "Buzon de Sugerencias .lnk",
    "Web.url"
)

foreach ($Archivo in $Archivos) {
    $RutaCompleta = Join-Path $Ruta $Archivo
    if (Test-Path $RutaCompleta) {
        Remove-Item $RutaCompleta -Force -ErrorAction SilentlyContinue
    }
}
```
