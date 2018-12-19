---
title: 使用zShell
tags:
  - shell
url: 128.html
id: 128
categories:
  - 开发工具
date: 2016-07-20 11:11:26
---

1.安装： sudo yum install zsh 或 sudo apt-get install zsh 2.将zsh作为默认shell： chsh -s /usr/local/bin/zsh 后面的路径可能因为系统的原因有所不同，可以使用 which zsh 查看路径 3.安装oh-my-zsh配置： wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh 有系统可能没有wget，如Mac，请自行安装