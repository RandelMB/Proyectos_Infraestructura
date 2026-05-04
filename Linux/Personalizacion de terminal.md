# PROMPT (PS1) PERSONALIZADO

```bash
nano ~/.bashrc                                   # editar configuración de bash
export PS1="\u@\h:\w\$ "                         # usuario@host:ruta$
export PS1="\[\e[32m\]\u@\h \[\e[34m\]\w \$\[\e[0m\] " # colores (verde usuario, azul ruta)
source ~/.bashrc                                 # aplicar cambios
echo $PS1                                        # verificar prompt actual
```
# COLORES Y ALIAS
```bash
nano ~/.bashrc                                   # abrir config
alias ll='ls -lah --color=auto'                 # listado detallado
alias la='ls -A'                                # listar ocultos
alias l='ls -CF'                                # formato columnas
export LS_COLORS='di=34:fi=0:ln=36:pi=33:so=35:bd=33:cd=33:or=31' # colores ls
source ~/.bashrc                                 # recargar
alias                                             # verificar alias activos
```
# HISTORIAL Y AUTOCOMPLETADO
```bash
nano ~/.bashrc                                   # editar config
HISTSIZE=10000                                   # tamaño historial
HISTFILESIZE=20000                               # tamaño archivo historial
HISTCONTROL=ignoredups:erasedups                # evitar duplicados
shopt -s histappend                              # no sobrescribir historial
bind '"\e[A": history-search-backward'          # buscar con flecha arriba
bind '"\e[B": history-search-forward'           # buscar con flecha abajo
source ~/.bashrc                                 # aplicar cambios
history | tail                                   # verificar historial
```
# TEMAS Y PLUGINS ZSH
```bash
nano ~/.zshrc                                    # editar config zsh
ZSH_THEME="agnoster"                            # cambiar tema
plugins=(git sudo zsh-autosuggestions zsh-syntax-highlighting) # plugins
source ~/.zshrc                                  # aplicar cambios
echo $ZSH_THEME                                  # verificar tema
```
# INSTALAR PLUGINS ZSH
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions # autosugerencias
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting # resaltado
nano ~/.zshrc                                    # agregar plugins
source ~/.zshrc                                  # aplicar cambios
```

# FUENTES Y POWERLINE

```bash
apt install -y fonts-powerline                  # instalar fuentes powerline
fc-cache -fv                                     # actualizar cache fuentes
echo $TERM                                       # verificar terminal
```

# STARSHIP (PROMPT AVANZADO)

```bash
curl -sS https://starship.rs/install.sh | sh     # instalar starship
echo 'eval "$(starship init bash)"' >> ~/.bashrc # activar en bash
echo 'eval "$(starship init zsh)"' >> ~/.zshrc   # activar en zsh
source ~/.bashrc                                 # aplicar cambios
starship --version                               # verificar instalación
```

# CONFIGURAR STARSHIP

```bash
mkdir -p ~/.config                               # crear directorio config
nano ~/.config/starship.toml                     # editar configuración
starship preset nerd-font-symbols -o ~/.config/starship.toml # preset base
source ~/.bashrc                                 # recargar
```

# CAMBIAR NOMBRE HOST

```bash
hostnamectl set-hostname servidor-linux          # cambiar hostname
hostnamectl                                      # verificar cambio
```

# BANNER Y MOTD

```bash
nano /etc/motd                                   # editar mensaje bienvenida
nano /etc/issue                                  # mensaje antes login
echo "Bienvenido a servidor" > /etc/motd         # escribir mensaje
cat /etc/motd                                    # verificar contenido
```

# LIMPIEZA Y OPTIMIZACIÓN

```bash
apt install -y neofetch                          # info sistema visual
neofetch                                         # mostrar info
clear                                            # limpiar pantalla
reset                                            # reiniciar terminal
```

# TERMINAL MULTIPLEXOR (TMUX)

```bash
apt install -y tmux                              # instalar tmux
tmux                                             # iniciar sesión
tmux new -s sesion                               # nueva sesión
tmux attach -t sesion                            # reconectar sesión
tmux ls                                          # listar sesiones
```
# INSTALAR ZSH + OH-MY-ZSH
```bash
apt update && apt install -y zsh git curl       # instalar dependencias
chsh -s $(which zsh)                            # cambiar shell por defecto
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" # instalar oh-my-zsh
zsh --version                                   # verificar instalación
```
## LISTAR TEMAS DISPONIBLES (OH-MY-ZSH)
```bash
ls ~/.oh-my-zsh/themes                         # listar todos los temas instalados
ls ~/.oh-my-zsh/themes | wc -l                 # contar cantidad de temas
ls ~/.oh-my-zsh/themes | sed 's/.zsh-theme//g' # limpiar extensión
```
## PREVISUALIZAR TEMAS RÁPIDO
```bash
for theme in ~/.oh-my-zsh/themes/*.zsh-theme; do # iterar temas
  name=$(basename "$theme" .zsh-theme)           # obtener nombre
  echo "Tema: $name"                            # mostrar nombre
  ZSH_THEME="$name" zsh -i -c "exit"             # cargar temporal
done
```
## CAMBIAR TEMA MANUAL
```bash
nano ~/.zshrc                                   # editar config
ZSH_THEME="agnoster"                           # definir tema
source ~/.zshrc                                 # aplicar cambios
echo $ZSH_THEME                                 # verificar tema activo
```
## CAMBIO AUTOMÁTICO ALEATORIO
```bash
nano ~/.zshrc                                   # editar config
ZSH_THEME="random"                             # activar random
source ~/.zshrc                                 # aplicar cambios
echo $ZSH_THEME                                 # verificar modo random
```
## FIJAR LISTA DE TEMAS ALEATORIOS
```bash
nano ~/.zshrc                                   # editar config
ZSH_THEME_RANDOM_CANDIDATES=("agnoster" "robbyrussell" "powerlevel10k") # lista personalizada
ZSH_THEME="random"                             # activar random
source ~/.zshrc                                 # aplicar cambios
```
## INSTALAR POWERLEVEL10K (MEJOR OPCIÓN)
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k # clonar tema
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc                 # activar tema
source ~/.zshrc                                                                  # aplicar cambios
p10k configure                                                                   # asistente configuración
```
## VERIFICAR TEMA ACTIVO
```bash
echo $ZSH_THEME                                 # mostrar tema configurado
grep ZSH_THEME ~/.zshrc                         # validar en archivo
```