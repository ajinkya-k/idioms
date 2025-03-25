# Useful Shell Commands

## File sizes

When using `bash`, the following creates a list of files with their file sizes, and sort in ascending order of size.

```bash
du --human-readable -d1 | sort --human-numeric-sort
```
(Source: somewhere on stackoverflow. Will update when I find it again)

# Prompt coloring for macOS ZSH

The snippet below modifies the `zsh` prompt by setting a green color for the name (hardcoded to my name here), magenta for path (only shows current folder name), followed by git branch name (if any) color coded by status.
This is basically just a copy of [this template](https://gist.github.com/chrisnolet/d3582cd63eb3d7b4fcb4d5975fd91d04) by [Chris Nolet](https://gist.github.com/chrisnolet) but with slight modifications for my own purposees.
To use, add the following lines to `.zshrc`:

```zsh
autoload -Uz compinit && compinit
autoload -Uz add-zsh-hook
autoload -Uz vcs_info

add-zsh-hook precmd vcs_info

zstyle ':vcs_info:*' enable git
zstyle ':vcs_info:*' formats " %F{cyan}%c%u(%b)%f"
zstyle ':vcs_info:*' actionformats " %F{cyan}%c%u(%b)%f %a"
zstyle ':vcs_info:*' stagedstr "%F{green}"
zstyle ':vcs_info:*' unstagedstr "%F{red}"
zstyle ':vcs_info:*' check-for-changes true

zstyle ':vcs_info:git*+set-message:*' hooks git-untracked

+vi-git-untracked() {
  if git --no-optional-locks status --porcelain 2> /dev/null | grep -q "^??"; then
    hook_com[staged]+="%F{red}"
  fi
}

setopt PROMPT_SUBST
export PROMPT='%F{green}ajinkya%f:%F{magenta}%1~%f$vcs_info_msg_0_ %# '
```
