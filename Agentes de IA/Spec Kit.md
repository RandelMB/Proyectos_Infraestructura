# INSTALAR GITHUB SPEC KIT (WINDOWS CLI)
```bash
winget install --id Git.Git -e --source winget        # instalar Git oficial
winget install --id OpenJS.NodeJS.LTS -e              # instalar Node.js LTS
npm install -g @github/spec-kit                       # instalar Spec Kit global
spec-kit --version                                   # verificar instalación
```
# USO BÁSICO
```bash
spec-kit init mi-proyecto                            # inicializar proyecto
cd mi-proyecto                                       # entrar al directorio
spec-kit generate spec.yaml                          # generar spec base
spec-kit validate spec.yaml                          # validar spec
```
# INSTALAR DEPENDENCIAS (UBUNTU)
```bash
sudo apt update && sudo apt install -y git python3 python3-pip curl # instalar requisitos base
curl -Ls https://astral.sh/uv/install.sh | bash                     # instalar uv (package manager)
source ~/.bashrc                                                    # recargar entorno
uv --version                                                        # verificar uv
python3 --version                                                   # verificar python (>=3.11 recomendado)
git --version                                                       # verificar git
```
# INSTALAR SPEC KIT (CLI OFICIAL)
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git  # instalar CLI oficial
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git --system-certs   # usar certificados del sistema
source ~/.bashrc                                                    # Recargar Variables
specify --version                                                   # verificar instalación
specify check                                                       # validar entorno
```
# INTEGRAR CON CODEX (PROYECTO)
```bash
mkdir mi-proyecto && cd mi-proyecto                       # crear proyecto
git init                                                  # inicializar repo
specify init # inicializar estructura Spec Kit  
specify check # validar entorno nuevamente
specify init . --integration codex --script sh            # integrar con Codex CLI
ls -la .agents/skills                                     # verificar integración Codex
```
# USO CON CODEX
```bash
codex                                                     # iniciar Codex CLI
# dentro de Codex usar:
$speckit-specify                                          # crear especificación
$speckit-plan                                             # generar plan
$speckit-tasks                                            # generar tareas
```
# VERIFICACIÓN FINAL
```bash
specify check                                             # validar instalación completa
ls -la .specify/                                          # verificar estructura spec-kit
ls -la .agents/skills                                     # verificar comandos codex
```

# USAR EN CODEX
## RESUMEN
```bash
specify init .
Proyecto inicializado correctamente                     # Spec Kit listo para usar
.agents/skills instalado                                # Skills disponibles para Codex
Git habilitado por defecto                              # Cambiará en v0.10.0 a opt-in manual
Agregar .agents/ a .gitignore                           # Evita filtrar credenciales/tokens
```
## COMANDOS PARA USAR EN CODEX
```bash
$speckit-constitution   # Definir principios/reglas del proyecto
$speckit-specify        # Crear especificación base
$speckit-plan           # Generar plan de implementación
$speckit-tasks          # Crear tareas accionables
$speckit-implement      # Ejecutar implementación
```
## COMANDOS PARA USAR EN CODEX (SKILLS OPCIONALES)
```bash
$speckit-clarify        # Resolver ambigüedades antes del plan
$speckit-checklist      # Validar calidad y consistencia
$speckit-analyze        # Analizar alineación entre artefactos
```
## RECOMENDADO
```bash
echo ".agents/" >> .gitignore    # Proteger credenciales/agentes
codex                            # Abrir Codex dentro del proyecto
ls -la .agents/skills            # Verificar instalación de skills
git status                       # Verificar estado del repositorio
```
