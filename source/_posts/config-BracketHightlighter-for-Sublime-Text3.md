title: Sublime Text3配置BracketHighlighter插件 
date: 2016-03-20 21:08:29
tags: sublime,BracketHighlighter
---

BracketHighlighter是一款括号匹配高亮显示的sublime插件
下面是官网上的一个截图
![BracketHighlighter](http://facelessuser.github.io/BracketHighlighter/images/Example1.png)

今天就介绍一下这个插件在mac下如何安装到Sublime Text3下
<!-- more -->
# 1.安装
----
通过Package Control，搜索BracketHighlighter安装即可。

# 2.BracketHighlighter配置
----
Preference > Package Settings > BracketHighlighter > Bracket Settings – User

对应的关系：
{} － curly
() － round
[] － square
<> － angle
“” － quote

style: solid、underline、outline、highlight
在这个文件为调整样式，但还是无法达到上图中的效果。

# 3.进一步修改配置

1. 在Finder里打开`/Applications/Sublime Text.app/Contents/MacOS/Packages`目录(这个是ST3自带的Packges默认目录)，找到`Color Scheme – Default.sublime-package`文件2. 
2. 修改文件打开方式，选中文件command+i,修改文件名为`Color Scheme - Default.zip`
![](/images/color scheme zip.png)

3. 解压文件，在解压的文件中找到`Monokai.tmTheme`使用vim打开按如下修改
```xml
<dict>
    <key>name</key>
    <string>Bracket Default</string>
    <key>scope</key>
    <string>brackethighlighter.default</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#FFFFFF</string>
        <key>background</key>
        <string>#A6E22E</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Unmatched</string>
    <key>scope</key>
    <string>brackethighlighter.unmatched</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#FFFFFF</string>
        <key>background</key>
        <string>#FF0000</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Curly</string>
    <key>scope</key>
    <string>brackethighlighter.curly</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#FF00FF</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Round</string>
    <key>scope</key>
    <string>brackethighlighter.round</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#E7FF04</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Square</string>
    <key>scope</key>
    <string>brackethighlighter.square</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#FE4800</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Angle</string>
    <key>scope</key>
    <string>brackethighlighter.angle</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#02F78E</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Tag</string>
    <key>scope</key>
    <string>brackethighlighter.tag</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#FFFFFF</string>
        <key>background</key>
        <string>#0080FF</string>
    </dict>
</dict>

<dict>
    <key>name</key>
    <string>Bracket Quote</string>
    <key>scope</key>
    <string>brackethighlighter.quote</string>
    <key>settings</key>
    <dict>
        <key>foreground</key>
        <string>#56FF00</string>
    </dict>
</dict>
```

4. 将上边的代码添加到Monokai.tmTheme中，和上边的dict并列排即可。
然后再降修改完成的文件重新压缩到`Color Scheme – Default.zip`
5. 选中`Color Scheme – Default.zip`command+i，将其改名为`Color Scheme - Default.sublime-package`
![](/images/color scheme.png)

这样重新打开sublime就可以看到效果了



