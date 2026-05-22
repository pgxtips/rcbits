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

## tmux bits
```
tssh() {
    if [[ $# -eq 0 ]]; then
        echo "Usage: tssh host1 [host2 ...]" >&2
        return 1
    fi

    local hosts=("$@")

    if [[ -n "$TMUX" ]]; then
        local window
        window=$(tmux new-window -P -F '#{session_name}:#{window_index}' "ssh ${hosts[0]}")
        for host in "${hosts[@]:1}"; do
            tmux split-window -t "$window" "ssh $host"
            tmux select-layout -t "$window" tiled
        done
        tmux select-layout -t "$window" tiled
        tmux select-window -t "$window"
    else
        local session="tssh-$$"
        tmux new-session -d -s "$session" "ssh ${hosts[0]}"
        for host in "${hosts[@]:1}"; do
            tmux split-window -t "$session" "ssh $host"
            tmux select-layout -t "$session" tiled
        done
        tmux select-layout -t "$session" tiled
        tmux attach-session -t "$session"
    fi
}
```
