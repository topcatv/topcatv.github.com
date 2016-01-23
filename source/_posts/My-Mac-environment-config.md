title: 我的MacBook系统及开发环境配置
date: 2016-01-23 22:09:21
tags: [mac]
categories: [mac]
---

MacBookPro已入手多时，现在也已用的比较熟练了，这里记录一下我的Mac的开发环境配置。下面将从几个方面记录一下板沙的成果。

1. 基础软件[HomeBrew](http://brew.sh/)、[iterm2](http://iterm2.com/)、[ohmyzsh](http://ohmyz.sh/)
2. 美化[powerline](https://github.com/powerline/powerline)
3. 系统工具Mackup
4. 开发工具MacVim或Vim、[Sublime Text 3](http://www.sublimetext.com/3)

<!-- more -->


# 安装基础软件
----
## 安装Homebrew
首先安装Homebrew，安装了Homebrew后就可以通过它来方便的安装其它的库了。(**注意：安装Homebrew的前提是我们先要在AppStore里安装XCode**)
``` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
get [cask](http://caskroom.io/)，cask可以帮助我们安装一些软件程序，比如Chrome
```sh
brew tap caskroom/cask
```
安装Chrome
```sh
brew cask install google-chrome
```
我这里还通过Homebrew安装了[mackup](https://github.com/lra/mackup)，python，nvm，gradle，maven，macvim

## 使用Homebrew安装iTerm2
----
```sh
brew cask install iterm2
```

## 安装ohmyzsh
----
```sh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

到此为至基础库和软件都已安装完成

# 安装powerline
----
详细安装[powerline](https://github.com/powerline/powerline)的方法要以参考他的[官方文档](https://powerline.readthedocs.org/en/latest/installation/osx.html)，我这里先简单的记录一下

首先要有python前面已通过Homebrew安装过了，下面通过[pip](https://pypi.python.org/pypi/pip/)安装。
```sh
pip install --user powerline-status
```
但是现在还是没有出来效果，我们还需要去安装[Powerline fonts](https://github.com/powerline/fonts)，从github上clone下面找到相应的字体文件在finder里双击就可以安装了。

安装好了后我们还要打开iterm2来做一些设置。

1. 打开iterm2
2. Preferences->Profiles,选择Defualt,General->Command这里填写`/bin/zsh`
3. 继续在Text里找到Regular Font和Non-ASCII font,我这里字体都设置为DejaVu Sans Mono for Powerline，大小12pt

到此重新打开iterm应该就可以看到效果了
![iterm2 & powerline](http://f.picphotos.baidu.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=c615912c9413b07eb9bd500d3cece01e/b3119313b07eca8058df2fea962397dda04483e0.jpg?referer=4b2184d2c91b9d16d3d0ae51edbe&x=.jpg)

# Mackup
----
[Mackup](https://github.com/lra/mackup)是一款备份你mac上配置的工具，它可以将配置文件link到指定的目录，你可以再把这个目录push到github上，这样你重装或换台机器时就可以很快的还原到你顺手的配置了。
具体安装和使用方法可以看它的github上的文档。

# Vim & Sublime Text 3
----
这两款编辑器也是非常好用的，vim的配置我也已经通过mackup备份到我的github上的，sublime现在用到的插件有

- AdvancedNewFile
- All Autocomplete
- Babel
- Better Completion
- BracketHighlighter
- Emmet
- jsFormat
- React ES6 Snippets
- ReactJS
- SideBarEnhancements
- SublimeCodeIntel

关于vim和Sublime的插件的安装于配置我会在将来的博客文章中再做介绍。



