title: 将vs code配置到zsh中命令行启动
date: 2018-02-14 20:48:47
categories: [mac]
tags: [mac, zsh, vs code]
---

我们在mac上使用Visual Studio Code时希望在zsh中通过命令快速打开一个文件或文件夹如何做到？
<!-- more -->

我们可以在我们的`.zshrc`文件中加入如下代码：
``` bash
function code {
    if [[ $# = 0 ]]
    then
        open -a "Visual Studio Code"
    else
        local argPath="$1"
        [[ $1 = /* ]] && argPath="$1" || argPath="$PWD/${1#./}"
        open -a "Visual Studio Code" "$argPath"
    fi
}
```
这样在zsh中我们可以使用下面的命令打开 **Visual Studio Code**

> `code` – 打开 Visual Studio Code
> `code .` – 在 Visual Studio Code 中打开当前目录
> `code somefile` – 在 Visual Studio Code 中打开somefile