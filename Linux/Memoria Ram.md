


```shell
# Extratraer info de Memoria o volcado

PS E:\DESCARGAS\SETUP> .\winpmem_mini_x64_rc2.exe

Acces Data FTK Imager
```

```shell

# Instalar Volatility
py -3 -m pip install volatility3

# ver la verison de vol
vol --help

E:\Programas\Python.3.14\Scripts\vol.exe -f 
"C:\Users\Randel\AppData\Local\Microsoft\Windows\TaskManager\LiveKernelDumps\livedump.DMP" windows.info

E:\Programas\Python.3.14\Scripts\vol.exe -s C:\Symbols -f "C:\Users\Randel\Desktop\archivo.raw" windows.info

```

```python
PS C:\Windows\System32> E:\Programas\Python.3.14\Scripts\vol.exe -f "C:\Users\Randel\AppData\Local\Microsoft\Windows\TaskManager\LiveKernelDumps\livedump.DMP" windows.info
Volatility 3 Framework 2.26.2
Progress:  100.00               PDB scanning finished
Unsatisfied requirement plugins.Info.kernel.layer_name:
Unsatisfied requirement plugins.Info.kernel.symbol_table_name:

A translation layer requirement was not fulfilled.  Please verify that:
        A file was provided to create this layer (by -f, --single-location or by config)
        The file exists and is readable
        The file is a valid memory image and was acquired cleanly

A symbol table requirement was not fulfilled.  Please verify that:
        The associated translation layer requirement was fulfilled
        You have the correct symbol file for the requirement
        The symbol file is under the correct directory or zip file
        The symbol file is named appropriately or contains the correct banner

Unable to validate the plugin requirements: ['plugins.Info.kernel.layer_name', 'plugins.Info.kernel.symbol_table_name']
PS C:\Windows\System32>
```

"C:\Users\Randel\AppData\Local\Microsoft\Windows\TaskManager\LiveKernelDumps\livedump.DMP"