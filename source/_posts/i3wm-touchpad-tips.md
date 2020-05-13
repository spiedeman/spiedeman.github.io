---
title: Linux下配置触摸板手势操作
category: i3
tags:
  - 触摸板
  - 手势操作
abbrlink: 35c03362
date: 2020-05-13 18:42:26
---

MacOS的触摸板手势体验没得说，在Linux下也可以借助一些软件达到相近的体验。不过最想要的**三指取词**还不知到如何实现😠。

首先配置触摸板的驱动程序为`libinput`，具体操作这里不谈。借助软件[libinput-gestures](https://github.com/bulletmark/libinput-gestures)可以拓展触摸板**三/四指手势**以及**缩放**操作。

i3桌面下，窗口有两种：**平铺**以及**浮动**。工作区的概念类比MacOS中的虚拟桌面，便签区的效果类似**窗口最小化**。

### 功能表
|      | `up`                 | `down`                 | `left`           | `right`        | 
|:-----:|:---------------------:|:-----------------------:|:-----------------:|:---------------:|
| 三指 | 切换到上方窗口       | 切换到下方窗口        | 切换到左侧窗口   | 切换到右侧窗口 | 
| 四指 | 将浮动窗口移到便签区 | 循环切换便签区内的窗口 | 切换到左侧工作区 | 切换到右侧工作区| 

如果当前获得焦点的窗口是浏览器，则**三指**的手势功能为

|      | `up` | `down` | `left`        | `right`       | 
|:-----:|:-----|:-------:|:--------------:|:--------------:|
| 三指 | 前进 | 后退   | 切换到左侧tab | 切换到右侧tab | 

以及另外四种手势

|      | `leftup`           | `leftdown`     | `rightup`      |`rightdown`     | 
|:-----:|:-------------------:|:---------------:|:---------------:|:----------------:|
| 三指 | 打开最近关闭标签页 | 关闭当前标签页 | 左移当前便签页 |右移当前标签页 | 

### libinput-gestures 配置文件
`libinput-gestures`的配置文件位于`~/.config/libinput-gestures.conf`。其中涉及到上述功能的配置为

```conf
# 三指手势
gestures swipe left  3 $HOME/Program/bin/move_window_or_tab
gestures swipe right 3 $HOME/Program/bin/move_window_or_tab right
gestures swipe up    3 $HOME/Program/bin/move_window_or_tab up
gestures swipe down  3 $HOME/Program/bin/move_window_or_tab down

# 四指手势
gestures swipe left  4 i3-msg workspace prev
gestures swipe right 4 i3-msg workspace next
gestures swipe up    4 $HOME/Program/bin/show_or_hide_scratchpad up
gestures swipe down  4 xdotool key super+shift+minus

# 浏览器窗口中三指手势
gestures swipe left_up    3 $HOME/Program/bin/move_window_or_tab left_up
gestures swipe left_down  3 $HOME/Program/bin/move_window_or_tab left_down
gestures swipe right_up   3 $HOME/Program/bin/move_window_or_tab right_up
gestures swipe right_down 3 $HOME/Program/bin/move_window_or_tab right_down
```



### 实现脚本
`libinput-gestures`能捕捉手势，但是将手势与操作绑定需要通过另一个能实现模拟按键操作的软件`xdotool`。
由于**三指**手势对应的功能较为复杂，需要区分窗口内是否为浏览器，且涉及的主要功能为切换窗口，因而统一用一个脚本`move_window_or_tab`实现。

```bash
#!/usr/bin/env bash

# 获取窗口名称
WM_NAME="$(xdotool getactivewindow getwindowname)"
# 获取手势方向
DIRECTION=${1:-left}

if [[ $WM_NAME =~ 'Firefox' ]]; then
    if [[ $DIRECTION == 'left' ]]; then
        xdotool key control+Page_Up
    elif [[ $DIRECTION == 'right' ]]; then
        xdotool key control+Page_Down
    # ...
    fi
else
    # 普通i3窗口
    # Key number: H-43, J-44, K-45, L-46, ;-47
    if [[ $DIRECTION == 'left' ]]; then
        xdotool key super+44
    elif [[ $DIRECTION == 'right' ]]; then
        xdotool key super+47
    # ...
    fi
fi
```

四指切换工作区的命令很简单，不需要借助脚本实现。但是涉及到**便签区窗口**的功能无法用一行命令搞定，因此写了一个脚本`show_or_hide_scratchpad`。
```bash
# 获取窗口ID
wm_ID="$(xdotool getactivewindow)"

# 获取手势方向
DIRECTION=${1:-up}

# 判断获得焦点的窗口是否为浮动窗口
function is_floating() {
    if xprop -id $1 | grep -i float > /dev/null 2>&1; then
        echo 'yes'
    else
        echo 'no'
    fi
}

# 尝试获取浮动窗口的ID
if [[ $is_floating $wm_ID ]]; then
    xdotool key super+space
    wm_ID="$(xdotool getactivewindow)"
fi

# 核心逻辑部分
if [[ $(is_floating $wm_ID) == yes ]]; then
    if [[ $DIRECTION == 'up' ]]; then
        xdotool key super+minus
    elif [[ $DIRECTION == 'down' ]]; then
        xdotool key super+shift+minus
    fi
fi
```

