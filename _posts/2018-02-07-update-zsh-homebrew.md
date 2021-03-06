---
layout: post
title: 'Update ZSH Homebrew'
date: 2018-02-07 09:11
comments: true
categories: Terminal
description: '更新ZSH'
tags: Terminal
reference:
  name:
    - oh-my-zsh-issues
    - updatezsh
  link:
    - https://github.com/robbyrussell/oh-my-zsh/issues/1984
    - https://stackoverflow.com/questions/17648621/how-do-i-update-zsh-to-the-latest-version
---

# check the zsh info
更新`ZSH`
```c
  upgrade_oh_my_zsh
```
如果更新過曾發生以下問題
```c
upgrade_oh_my_zsh
Updating Oh My Zsh
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.
There was an error updating. Try again later?
```
請使用以下這行進行修補
```c
  cd "$ZSH" && git stash && upgrade_oh_my_zsh
```
詳細編輯zsh file(brew info zsh) `# install zsh`
brew install --without-etcdir zsh
# add shell path `sudo vim /etc/shells`
add the following line into the very end of the file(/etc/shells)
`/usr/local/bin/zsh`
# change default shell
`chsh -s /usr/local/bin/zsh`