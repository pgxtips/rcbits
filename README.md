# rcbits

## FZF stuff, to allow fzf tab completion
```
[ -f /usr/share/bash-completion/completions/fzf ] && source /usr/share/bash-completion/completions/fzf
[ -f /usr/share/doc/fzf/examples/key-bindings.bash ] && source /usr/share/doc/fzf/examples/key-bindings.bash
_fzf_compgen_path() { fdfind --hidden --follow --exclude .git . "$1"; }
_fzf_compgen_dir()  { fdfind --type d --hidden --follow --exclude .git . "$1"; }
export FZF_COMPLETION_TRIGGER="~~"
export FZF_CTRL_T_COMMAND='fdfind --hidden --follow --exclude .git .'
```
need to install fzf & fd-find

```
sudo apt install fzf
sudo apt install fd-find
```

## vim bits
- requires: fzf, ripgrep & vim
```
_live_grep() {
        local result
        result=$(echo -n  \
                | fzf --disabled --bind "change:reload:rg -n -i {q} || true" \
                | awk 'BEGIN {FS=":"} { print "+" $2 " " $1 }' \
        )
        [ -n "$result" ] && vim $result
}
bind -x '"\C-g": _live_grep'
```
