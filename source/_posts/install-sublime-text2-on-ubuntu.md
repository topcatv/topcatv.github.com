title: 在Ubuntu上安装Sublime Text2并解决无法使用fcitx中文输入法问题
date: 2013-10-08 11:05:29
categories: [ubuntu]
tags: [ubuntu, ficxt, sublime text2]
---

### 第一步 下载安装
----
到Sublime Text官网去[下载](http://www.sublimetext.com/2)安装包（我这里使用的是64位的）并解压
``` bash 解压 start:51 mark:51
tar xvf Sublime\ Text\ 2.0.2\ x64.tar.bz2
```
<!-- more -->

### 第二步 移动
----
将解压后的目录移动到/opt/目录下
``` bash
sudo mv Sublime\ Text\ 2 /opt/
```

### 第三步 创建符号链接
----
创建符号链接到/usr/bin/下
``` bash
sudo ln -s /opt/Sublime\ Text\ 2/sublime_text /usr/bin/sublime
```

### 第四步 创建启动器
----
创建并编辑/usr/share/applications/sublime.desktop文件
``` bash
sudo vim /usr/share/applications/sublime.desktop
```

文件内容如下
```
[Desktop Entry]
Version=1.0
Name=Sublime Text 2
# Only KDE 4 seems to use GenericName, so we reuse the KDE strings.
# From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
GenericName=Text Editor

Exec=sublime
Terminal=false
Icon=/opt/Sublime Text 2/Icon/48x48/sublime_text.png
Type=Application
Categories=TextEditor;IDE;Development
X-Ayatana-Desktop-Shortcuts=NewWindow

[NewWindow Shortcut Group]
Name=New Window
Exec=sublime -n
TargetEnvironment=Unity
```

### 第五步 解决中文输入法不能使用的问题
----
- 创建文件sublime-imfix.c
``` bash
vim sublime-imfix.c
```
文件内容如下
``` c
/**
sublime-imfix.c
Use LD_PRELOAD to interpose some function to fix sublime input method support for linux.
By Cjacker Huang <jianzhong.huang at i-soft.com.cn>

gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
LD_PRELOAD=./libsublime-imfix.so sublime_text
*/

#include <gtk/gtk.h>
#include <gdk/gdkx.h>
typedef GdkSegment GdkRegionBox;

struct _GdkRegion
{
  long size;
  long numRects;
  GdkRegionBox *rects;
  GdkRegionBox extents;
};

GtkIMContext *local_context;

void
gdk_region_get_clipbox (const GdkRegion *region,
            GdkRectangle    *rectangle)
{
  g_return_if_fail (region != NULL);
  g_return_if_fail (rectangle != NULL);

  rectangle->x = region->extents.x1;
  rectangle->y = region->extents.y1;
  rectangle->width = region->extents.x2 - region->extents.x1;
  rectangle->height = region->extents.y2 - region->extents.y1;
  GdkRectangle rect;
  rect.x = rectangle->x;
  rect.y = rectangle->y;
  rect.width = 0;
  rect.height = rectangle->height; 
  //The caret width is 2; 
  //Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.
  if(rectangle->width == 2 && GTK_IS_IM_CONTEXT(local_context)) {
        gtk_im_context_set_cursor_location(local_context, rectangle);
  }
}

//this is needed, for example, if you input something in file dialog and return back the edit area
//context will lost, so here we set it again.

static GdkFilterReturn event_filter (GdkXEvent *xevent, GdkEvent *event, gpointer im_context)
{
    XEvent *xev = (XEvent *)xevent;
    if(xev->type == KeyRelease && GTK_IS_IM_CONTEXT(im_context)) {
       GdkWindow * win = g_object_get_data(G_OBJECT(im_context),"window");
       if(GDK_IS_WINDOW(win))
         gtk_im_context_set_client_window(im_context, win);
    }
    return GDK_FILTER_CONTINUE;
}

void gtk_im_context_set_client_window (GtkIMContext *context,
          GdkWindow    *window)
{
  GtkIMContextClass *klass;
  g_return_if_fail (GTK_IS_IM_CONTEXT (context));
  klass = GTK_IM_CONTEXT_GET_CLASS (context);
  if (klass->set_client_window)
    klass->set_client_window (context, window);

  if(!GDK_IS_WINDOW (window))
    return;
  g_object_set_data(G_OBJECT(context),"window",window);
  int width = gdk_window_get_width(window);
  int height = gdk_window_get_height(window);
  if(width != 0 && height !=0) {
    gtk_im_context_focus_in(context);
    local_context = context;
  }
  gdk_window_add_filter (window, event_filter, context); 
}
```

- 编译sublime-imfix.c
安装必要的编译包
```
sudo apt-get install build-essential
sudo apt-get install libgtk2.0-dev
```

编译
```
gcc -shared -o libsublime-imfix.so sublime-imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
```

- 将libsublime-imfix.so移动到/usr/lib/下
```
sudo mv libsublime-imfix.so /usr/lib/
```

- 修改启动器文件内容
```
[Desktop Entry]
Version=1.0
Name=Sublime Text 2
# Only KDE 4 seems to use GenericName, so we reuse the KDE strings.
# From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
GenericName=Text Editor

Exec=bash -c 'LD_PRELOAD=/usr/lib/libsublime-imfix.so sublime'
Terminal=false
Icon=/opt/Sublime Text 2/Icon/48x48/sublime_text.png
Type=Application
Categories=TextEditor;IDE;Development
X-Ayatana-Desktop-Shortcuts=NewWindow

[NewWindow Shortcut Group]
Name=New Window
Exec=bash -c 'LD_PRELOAD=/usr/lib/libsublime-imfix.so sublime' -n
TargetEnvironment=Unity
```

OK到此fcitx输入法可用

##### 参考文章
- [How to install Sublime Text 2 on Ubuntu 12.04 (Unity)](http://www.technoreply.com/how-to-install-sublime-text-2-on-ubuntu-12-04-unity/)
- [Ubuntu系统下Sublime Text 2中fcitx中文输入法的解决方法](http://my.oschina.net/wugaoxing/blog/121281?p=2)