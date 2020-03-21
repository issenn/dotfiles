# dotfiles

My dotfiles controlled by GNU stow following [this way](https://farseerfc.me/using-gnu-stow-to-manage-your-dotfiles.html)

## Requirements


```sh
brew install zsh
```
```sh
sudo vim /etc/shells
```
Add
```
/usr/local/bin/bash
/usr/local/bin/zsh
```
```sh
chsh -s /usr/local/bin/zsh
```

```sh
sudo vim /etc/paths
```
Add
```
/usr/local/sbin
```

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

### git-config
```sh
stow git-config
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
