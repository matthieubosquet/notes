# Git

> Git is a free and open source distributed version control system...
> – https://git-scm.com/


## Install

```sh
$ brew install git
```

## Config

```sh
$ cat <<EOF > ~/.gitconfig
[user]
	name = Jon Doe
	email = jd@example.org
	signingkey = 28XXADC389990236
[gpg]
	program = gpg
[commit]
	gpgsign = true
[pull]
	rebase = true
[push]
	autoSetupRemote = true
[tag]
	gpgsign = true
[alias]
	c = commit -m
  cl = clone
	p = push
  pl = pull
	l = log
[init]
	defaultBranch = main

EOF
```

## Global ignore

Globally ignore environment specific things (``):

```sh
$ cat <<EOF > ~/.config/git/ignore
.DS_STORE
.vscode
.idea
*.iml
*.local

EOF
```
