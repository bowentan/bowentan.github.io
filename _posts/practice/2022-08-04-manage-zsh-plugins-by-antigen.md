---
title: Manage Zsh plugins by Antigen
category: practice
tags: zsh antigen plugin
date: 2022-08-04 23:19:00 +0800
---

# Overview
As I shared in the post [Awesome Bash Tools]({% post_url practice/2022-07-24-awesome-bash-tools %}), the Zsh supports many useful plugins and I shared the installation methods by `git clone` and setting up manually, which may not be straightforward. When migrating to another server, it will save our life if a single `.zshrc` is enough.

Here, I will share another method to manage and install Zsh plugins more efficiently, with `antigen`.

# Antigen
`antigen` is a popular Zsh plugin manager which provides an easy-to-install way to add/remove Zsh plugins. Suppose you have already has oh-my-zsh installed (if not, go to post "Awesome Bash Tools" or [here](https://ohmyz.sh/)), `antigen` can be installed by a single `curl`:
```console
$ curl -L git.io/antigen > ~/antigen.zsh
```
or put it wherever you like. Then you can just add your plugins by adding corresponding `antigen bundle` lines in `.zshrc`. The typical `.zshrc` will be like this:
```zsh
source /path/to/antigen.zsh

# Load the oh-my-zsh's library.
antigen use oh-my-zsh

# Load the plugins here
antigen bundle git

# Load the theme that you want to use
antigen theme robbyrussell

# Tell Antigen that you're done.
antigen apply
```
This is a simple configuration including only one plugin `git` and a theme `robbyrussell`. You can modify `.zshrc` to be what you like and after modification, run `source ~/.zshrc` to install your plugins and take them into effect.

My `.zshrc` including the plugins that I shared before is given as follows:
```zsh
source $HOME/.antigen.zsh

# Load the oh-my-zsh's library.
antigen use oh-my-zsh

# Bundles from the default repo (robbyrussell's oh-my-zsh).
antigen bundle git
antigen bundle heroku
antigen bundle pip
antigen bundle lein
antigen bundle command-not-found

# My favorite plugins
antigen bundle zsh-users/zsh-syntax-highlighting
antigen bundle agkozak/zsh-z
antigen bundle zsh-users/zsh-autosuggestions

# Awesome theme.
antigen theme romkatv/powerlevel10k

# Tell Antigen that you're done.
antigen apply
```
There is one thing to be noticed that it's possible that after you sources `.zshrc`, the plugins are installed successfully but the prompt doesn't return to what it should be. In that case, just `exit` (yes, you can issue commands here still...) or close the window directly and login again, then it could be OK. I don't know what happens here so I can just give a trivial solution..

And another thing is that after installing plugins, `~/.antigen` directory will be created. If you come across an error involving `compaudit`, it results from the permissions of `~/.antigen` directory and its children and sometimes there will also another directories causing such an error. In that case, issue the command `compaudit` to check which directories are the sources of the error. Once you find them, change their permissions to `755` or remove the group/other writing permissions:
```console
$ chmod 755 -R ~/.antigen <other directory you find>
```