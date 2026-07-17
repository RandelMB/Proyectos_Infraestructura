
# INSTALAR GITHUB COPILOT CLI (WINDOWS 11)

```bash
winget install GitHub.cli                # instalar GitHub CLI oficial
gh --version                            # verificar instalación

gh extension install github/gh-copilot  # instalar extensión Copilot CLI
gh copilot --help                       # verificar comandos disponibles

gh auth login                           # autenticar con cuenta GitHub
gh copilot auth login                   # autenticar Copilot (requiere suscripción activa)

gh copilot suggest "crear script bash backup"   # generar comando sugerido
gh copilot explain "ls -la"                    # explicar comando

gh extension list                       # verificar extensión instalada
```