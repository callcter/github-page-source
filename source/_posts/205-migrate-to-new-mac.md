---
title: 新 Mac 配置记录
---

## 软件安装

1. 科学上网工具 —— [ShadowSocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/releases) | [ShadowsocksX-NG-R](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases)

2. 重度依赖上网工具 —— [Chrome](https://www.google.com/intl/zh-CN/chrome/)

3. 用了都说好的终端 —— [iTerm2](https://iterm2.com/)

4. 系统开发依赖 —— Command Line Tools

```
xcode-select --install
```

5. 包管理器 —— Homebrew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

6. 最好用的 shell & 配置 —— zsh & oh-my-zsh

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

7. Vim

```
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
```

8. ruby 配置

9. nodejs 配置

10. python 配置

scrapy-splash
```
docker run -p 8050:8050 scrapinghub/splash
```

11. Xcode

12. Android Studio

13. docker

```
brew cask install docker
```

## 访问加速

1. npm

```
# 切换为淘宝源
npm config set registry https://registry.npm.taobao.org
```

2. homebrew

```
# 切换为清华源
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
brew update
# 复原
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git
brew update
```

3. GitHub

- [IPAddress.com](https://www.ipaddress.com/)
直接添加以下地址 hosts
- github.com
- http://assets-cdn.github.com
- http://github.global.ssl.fastly.net

4. Pip

源：
- 豆瓣 (douban) http://pypi.douban.com/simple/
- 阿里云 http://mirrors.aliyun.com/pypi/simple/
- 清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
- 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
- 中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

```
mkdir .pip
vi .pip/pip.conf
```

```
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

5. docker

daemon
```
{
  "registry-mirrors" : [
    "http://ovfftd6p.mirror.aliyuncs.com",
    "http://registry.docker-cn.com",
    "http://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries" : [
    "registry.docker-cn.com",
    "docker.mirrors.ustc.edu.cn"
  ],
  "debug" : true,
  "experimental" : true
}
```

## 配置同步

1. VS Code

settings sync 插件

## Finder

1. 在 Finder 标题栏显示完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES;killall Finder
# 恢复
defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder
```

2. 预览

https://github.com/sindresorhus/quick-look-plugins
