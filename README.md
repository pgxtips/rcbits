# rcbits

## FZF stuff, to allow fzf tab completion
```
_fzf_compgen_path() { fdfind --hidden --follow --exclude .git . "$1"; }
_fzf_compgen_dir()  { fdfind --type d --hidden --follow --exclude .git . "$1"; }
export FZF_COMPLETION_TRIGGER="~~"
[ -f /usr/share/bash-completion/completions/fzf ] && source /usr/share/bash-completion/completions/fzf
```
need to install fzf & fd-find

```
sudo apt install fzf
sudo apt install fd-find
```
