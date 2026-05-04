
# iNSTALAR CLAUDE
```bash
# INSTALAR CLAUDE CODE CLI (NODE GLOBAL)
apt update && apt install -y nodejs npm curl            # instalar dependencias
npm install -g @anthropic-ai/claude-code                # instalar CLI oficial Claude Code

# -------- CONFIGURAR API KEY --------
export ANTHROPIC_API_KEY="TU_API_KEY"                   # exportar clave API (temporal)
echo 'export ANTHROPIC_API_KEY="TU_API_KEY"' >> ~/.bashrc  # persistir variable

# -------- USO BASICO --------
claude --help                                           # ver ayuda
claude "explica este codigo"                            # prueba rápida

# -------- VERIFICACION FINAL --------
claude --version                                        # verificar instalación
echo $ANTHROPIC_API_KEY                                 # confirmar variable cargada
```