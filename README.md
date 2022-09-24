# dotfiles

My dotfiles controlled by GNU stow following [this way](https://farseerfc.me/using-gnu-stow-to-manage-your-dotfiles.html)

## Requirements


# check the zsh info
```sh
brew info zsh
```
# install zsh
```sh
brew install zsh
```
# add shell path
```sh
sudo vim /etc/shells
```
# add the following line into the very end of the file(/etc/shells)
```
/usr/local/bin/bash
/usr/local/bin/zsh
```
# change default shell
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
<!-- 
> 后续折腾
> [rcm](https://github.com/thoughtbot/rcm) -->