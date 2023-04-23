# The following lines were added by compinstall

zstyle ':completion:*' completer _complete _ignored
zstyle :compinstall filename '/home/warre/.zshrc'

autoload -Uz compinit && compinit
# End of lines added by compinstall
# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=1000
SAVEHIST=1000
setopt autocd notify
unsetopt beep
bindkey -e
# End of lines configured by zsh-newuser-install

function setup_git {
    # https://dev.to/cassidoo/customizing-my-zsh-prompt-3417
    # Setup Version Control System info for the prompt
    autoload -Uz vcs_info
    precmd_vcs_info() { vcs_info }
    precmd_functions+=( precmd_vcs_info )
    setopt PROMPT_SUBST
    
    # Adapted from https://stackoverflow.com/questions/49744179/zsh-vcs-info-how-to-indicate-if-there-are-untracked-files-in-git
    # Hook for untracked/unstaged/staged changes
    +vi-git-untracked() {
        if [[ $(git rev-parse --is-inside-work-tree 2> /dev/null) == 'true' ]] && [[ -n $(git status --porcelain) ]] then
            hook_com[misc]='%F{red}✗%f'
        fi
    }
    
    zstyle ':vcs_info:*' enable git
    zstyle ':vcs_info:git*+set-message:*' hooks git-untracked
    zstyle ':vcs_info:git:*' formats ' %F{blue}(%f%F{yellow}%b%f%m%F{blue})%f'
}

function setup_keybinds {
    # https://wiki.archlinux.org/title/Zsh#Key_bindings
    # https://wiki.archlinux.org/title/Zsh#Shift,_Alt,_Ctrl_and_Meta_modifiers
    # create a zkbd compatible hash;
    # to add other keys to this hash, see: man 5 terminfo
    typeset -g -A key
    
    key[Home]="${terminfo[khome]}"
    key[End]="${terminfo[kend]}"
    key[Insert]="${terminfo[kich1]}"
    key[Backspace]="${terminfo[kbs]}"
    key[Delete]="${terminfo[kdch1]}"
    key[Up]="${terminfo[kcuu1]}"
    key[Down]="${terminfo[kcud1]}"
    key[Left]="${terminfo[kcub1]}"
    key[Right]="${terminfo[kcuf1]}"
    key[PageUp]="${terminfo[kpp]}"
    key[PageDown]="${terminfo[knp]}"
    key[Shift-Tab]="${terminfo[kcbt]}"
    key[Control-Left]="${terminfo[kLFT5]}"
    key[Control-Right]="${terminfo[kRIT5]}"
    
    # setup key accordingly
    [[ -n "${key[Home]}"      ]] && bindkey -- "${key[Home]}"       beginning-of-line
    [[ -n "${key[End]}"       ]] && bindkey -- "${key[End]}"        end-of-line
    [[ -n "${key[Insert]}"    ]] && bindkey -- "${key[Insert]}"     overwrite-mode
    [[ -n "${key[Backspace]}" ]] && bindkey -- "${key[Backspace]}"  backward-delete-char
    [[ -n "${key[Delete]}"    ]] && bindkey -- "${key[Delete]}"     delete-char
    [[ -n "${key[Up]}"        ]] && bindkey -- "${key[Up]}"         up-line-or-history
    [[ -n "${key[Down]}"      ]] && bindkey -- "${key[Down]}"       down-line-or-history
    [[ -n "${key[Left]}"      ]] && bindkey -- "${key[Left]}"       backward-char
    [[ -n "${key[Right]}"     ]] && bindkey -- "${key[Right]}"      forward-char
    [[ -n "${key[PageUp]}"    ]] && bindkey -- "${key[PageUp]}"     beginning-of-buffer-or-history
    [[ -n "${key[PageDown]}"  ]] && bindkey -- "${key[PageDown]}"   end-of-buffer-or-history
    [[ -n "${key[Shift-Tab]}" ]] && bindkey -- "${key[Shift-Tab]}"  reverse-menu-complete
    [[ -n "${key[Control-Left]}"  ]] && bindkey -- "${key[Control-Left]}"  backward-word
    [[ -n "${key[Control-Right]}" ]] && bindkey -- "${key[Control-Right]}" forward-word
    
    # Finally, make sure the terminal is in application mode, when zle is
    # active. Only then are the values from $terminfo valid.
    if (( ${+terminfo[smkx]} && ${+terminfo[rmkx]} )); then
        autoload -Uz add-zle-hook-widget
        function zle_application_mode_start { echoti smkx }
        function zle_application_mode_stop { echoti rmkx }
        add-zle-hook-widget -Uz zle-line-init zle_application_mode_start
        add-zle-hook-widget -Uz zle-line-finish zle_application_mode_stop
    fi
}

setup_git
setup_keybinds

# Load plugins
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh

# Set prompt
PROMPT='%B%n:%F{blue}%1~/%f${vcs_info_msg_0_}%b $ '
RPROMPT='%B[%*]%b'

# Load aliases
source ~/.aliases

# Set path
export PATH="$HOME/.local/bin:$PATH"

# Set editor
export VISUAL=vim
export EDITOR="$VISUAL"

# Load zsh-syntax-highlighting
# Should be at the end of the file as per https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# Menu select tab complete
# https://unix.stackexchange.com/questions/470714/replicate-oh-my-zsh-directory-tab-completion-selection-with-arrow-keys
zstyle ':completion:*' menu select  