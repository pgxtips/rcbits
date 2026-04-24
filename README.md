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
