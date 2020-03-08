# dotfiles

My dotfiles controlled by GNU stow following [this way](https://farseerfc.me/using-gnu-stow-to-manage-your-dotfiles.html)

## Requirements

```sh
brew install stow
```

## Usage

### stow-config
```sh
stow stow-config
```

### .keep
```sh
stow .keep
```

`# Meslo LG M DZ Regular for Powerline`

### zsh-config
```sh
brew install coreutils grep
# brew install exa fzf
stow zsh-config
```
#### Install Zinit Module
```sh
brew install make gcc gawk
zinit module build
```

### tmux-config
```sh
brew install tmux
brew install reattach-to-user-namespace
stow tmux-config
```
