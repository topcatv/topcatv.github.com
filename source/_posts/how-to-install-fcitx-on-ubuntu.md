title: 怎样在ubuntu中安装fcitx输入法
date: 2013-10-07 16:29:40
categories: [ubuntu]
tags: [ubuntu,输入法,fcitx]
---

- 设置fcitx源
``` bash
sudo add-apt-repository ppa:fcitx-team/nightly
```
<!-- more -->

- 更新库
``` bash
sudo apt-get update
```

- 安装fcitx
``` bash
sudo apt-get install fcitx fcitx-config-gtk fcitx-sunpinyin fcitx-table-wubi
```

- 切换输入法
``` bash
im-switch -s fcitx
```

- 防止乱码安装字体
``` bash
sudo apt-get install ttf-arphic-uming
```