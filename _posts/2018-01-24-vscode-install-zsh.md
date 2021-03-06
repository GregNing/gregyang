---
layout: post
title: 'VSCode 安裝 zsh'
date: 2018-01-24 14:34
comments: true
categories: VsCode
tags: VsCode
reference:
  name:
    - ZSH
    - ZSH Theme
    - visual-studio-code-zsh
  link:
    - https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH
    - https://github.com/robbyrussell/oh-my-zsh
    - https://www.jazz321254.com/visual-studio-code-zsh/
---
安裝[ZSH](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)
我所使用的是macOS，輸入以下安裝
```
brew install zsh zsh-completions
```
以下指令接連安裝都不會出現問題
```
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
cd ..
rm -rf fonts
```
設定[Zsh Theme](https://github.com/robbyrussell/oh-my-zsh)
```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
zsh
source ~/.zshrc
exit
git clone git://github.com/tarruda/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```
編輯`.zshrc`加入source
```
vim .zshrc
source ~/.zsh/zsh-autosuggestions/autosuggestions.zsh
```
#### 使用Powerlevel9k
```
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
echo 'ZSH_THEME="powerlevel9k/powerlevel9k"' >> ~/.zshrc
zsh
source ~/.zshrc
```
開啟 VSCode 使用快捷鍵 command + , 編輯 “整合式終端機”
```
"terminal.integrated.fontFamily": "Source Code Pro for Powerline",
"terminal.integrated.fontSize": 12
```
exit重開 VScode
開啟 VSCode 要進入 ZSH畫面輸入`zsh`指令就會看到畫面變了