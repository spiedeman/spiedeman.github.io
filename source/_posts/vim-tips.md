---
title: Vim 技巧
mathjax: false
toc: true
category:
  - Vim
tags:
  - Vim
abbrlink: 4131642440
date: 2019-02-18 10:11:19
---

## 用法
### 自定义映射

#### 正确使用option/Alt键
`option`键在`macOS`下作为组合按键之一，对应于PC、Linux上的`Alt`键，但是功能不太一样。`Mac`上字母及一些标点符号与`option`组合会输出`unicode`字符。
原因在于按下按下`option`键发送的不是`Escape
Sequence`。因此在`vim,emacs`等终端软件中，没法直接使用`Alt`键定义映射。

找到原因就好办，解决方法分为两步：
1. 在终端中修改`option/Alt`键的行为
2. 在`vim,emacs`等运行于终端下的软件中进行可能必要的设置

以终端程序`kitty`和`alacritty`为例，具体讨论解决方法中的第一步如何操作。
- kitty
  - 打开配置文件`kitty.conf`
  - 找到选项`macos_option_as_alt no`，将`no`改为`yes`
- alacrity
  - 打开配置文件`alacrity.yml`
  - 定位到`key_bindings`
  - 添加需要的键位设置，例如 `- { key: x, mods: Alt, chars: "\x1bx" }`

以运行在终端下的`vim`为例，给出第二步中需要的具体操作。
```vim
function! Terminal_MetaMode(mode)
    set ttimeout
    if $TMUX != ''
        set ttimeoutlen=30
    elseif &ttimeoutlen > 80 || &ttimeoutlen <= 0
        set ttimeoutlen=80
    endif
    if has('nvim') || has('gui_running') || $TERM_PROGRAM =~? 'iTerm2'
        return 
    endif
    " 👆表示只有某些终端下的vim需要👇的设置
    function! s:metacode(mode, key)
        " 这个函数的作用是告诉vim，<M-x>的键盘序列码是多少
        " 这样vim将按照ttimeoutlen的设置来检查是否超时
        " 如果用 `noremap <ESC>x <M-x>` 然后 `noremap <M-x> ...`的方式，
        " 则会使用timeoutlen来检查是否超时
        " 一般timeoutlen设置的相对更大，如果用它更容易误操作，降低使用体验
        if a:mode == 0
            exec "set <M-".a:key.">=\e".a:key
        else
            "👇这条需要对终端进行更针对性的设置，写本节时只想到在alacrity中设置会容易些
            exec "set <M-".a:key.">=\e]{0}".a:key."~"
        endif
    endfunction
    " 针对alacrity，还需设置几个功能键
    if $TERM_PROGRAM =~? 'alacritty'
        exec "set <F1>=\eOP"
        exec "set <F2>=\eOQ"
        exec "set <F3>=\eOR"
        exec "set <F4>=\eOS"
    endif
    for i in range(10)
        call s:metacode(a:mode, nr2char(char2nr('0') + i))
    endfor
    for i in range(26)
        call s:metacode(a:mode, nr2char(char2nr('a') + i))
        call s:metacode(a:mode, nr2char(char2nr('A') + i))
    endfor
    if a:mode != 0
        for c in [',', '.', '/'. ';', '[', ']', '{', '}']
            call s:metacode(a:mode, c)
        endfor
        for c in ['?', ':', '-', '_']
            call s:metacode(a:mode, c)
        endfor
    else
        for c in [',', '.', '/', ';', '{', '}']
            call s:metacode(a:mode, c)
        endfor
        for c in ['?', ':', '-', '_']
            call s:metacode(a:mode, c)
        endfor
    endif
endfunction

" 设置用户自定义命令
command! -nargs=0 -bang VimMetaInit call Terminal_MetaMode(<bang>0)
" buffer 读入后自动进行设置
augroup alt_key
    autocmd!
    autocmd BufReadPost * :VimMetaInit
augroup END
```


### 用户自定义事件
自定义了切换透明和背景的函数，并绑定了快捷键。同时主题栏使用的是[lightline](https://github.com/itchyny/lightline.vim)。
我为`dark`和`light`两种背景选取不同的`lightline`主题，为了使背景切换之后，主题栏的切换也生效，这里使用用户自定义事件来解决问题。
当然这不是唯一的方法。
```vim
function! ToggleBackground()
    " A lot of stuff is happening here.
    " 定义一个自定义事件
    doautocmd User ToggleBackgroundExit
endfunction

function! ToggleTransparent()
    " A lot of stuff is happening here.
    " 定义一个自定义事件
    doautocmd User ToggleTransparentExit
endfunction
```
现在可以在切换背景和透明度执行完成后做任何想做的事：
```vim
autocmd User ToggleBackgroundExit call lightline#enable()
```

### 插件
#### 插件管理器（vim-plug）
[vim-plug](https://github.com/junegunn/vim-plug)是一款异步插件管理器。具有安装速度快，延迟加载等特性。
这里要讨论的是刚发现的一个特性，可以让插件按照依赖关系进行加载。

栗子🌰，我有三款用于`markdown`的插件，分别是

- [`plasticboy/vim-markdown`](https://github.com/plasticboy/vim-markdown)
- [`jszakmeister/markdown2ctags`](https://github.com/jszakmeister/markdown2ctags)
- [`iamcco/markdown-preview.nvim`](https://github.com/iamcco/markdown-preview.nvim)

原本可以在每个插件后面，用`{'for':'markdown'}`使其仅在编辑`.md`文件时才加载。但我发现
这种方法对第二款插件无效。因此想到这里的办法。

`vim-plug`在调用函数`plug#load()`加载插件之后会以插件名定义一个用户自定义事件。可以利用这一事件触发对其有依赖的插件的调用。如下为具体的使用案例：

```vim
" 以文件类型作为加载的依据
Plug 'iamcco/markdown-preview.nvim', {'for': 'markdown', 'do': 'cd app & yarn install'}
  " 下面的插件无法按照文件类型进行加载
  " 故通过vim-plug 定义的用户自定义事件进行触发
  Plug 'plasticboy/vim-markdown', {'on' : []}
  Plug 'jszakmeister/markdown2ctags', {'on': []}
  augroup vimplug_load_for_markdown
    autocmd!
    autocmd User markdown-preview.nvim plug#load(
                \ 'vim-markdown',
                \ 'markdown2ctags',
                \ )
                \ | autocmd! vimplug_load_for_markdown
  augroup END
```

进一步，针对一个插件的所有配置可以做到这样：在插件被

- 加载前，不载入
- 加载后，自动载入

可以这样实现

```vim
" 仍然以上面的插件为例
" 在载入 vim-markdown之后自动加载相关配置。所有配置在函数SetVimmarkdown()之中
function! SetVimmarkdown()
    " all configurations for vim-markdown are at here
endfunction

augroup load_for_vimmarkdown
    autocmd!
    autocmd User vim-markdown call SetVimmarkdown() | autocmd!  load_for_vimmarkdown
augroup END
```

#### 中文输入法
`vim` 的模式切换与中文输入法并不总能够和谐共处。
|        | 英文 | 中文 |
|--------|------|------|
| normal | :)   | :(   |
| insert | :)   | :)   |

解决这个问题的方式大体上有两种，1、`vim`
提供中文输入法，那么这个问题就留给`vim`及其插件的开发者们去解决了；2、仍然使用外部中文输入法，如搜狗等，
那么关键在于以哪种方式自动切换输入法！

这里沿用第二种思路。尝试过如下几种方式：

1. 通过 vim 内置函数模拟按键操作来切换输入法。没有成功，但不排除可行性。
2. 调用外部程序进行输入法切换。成功，但是并不完美。

所以最终采取的方案是通过调用外部程序在进入和离开`insert`模式时只能切换输入法。实现方式具体看下面的`vim`配置。

```vim
" 定义缓冲区变量，使得可以对每个缓冲区单独监控输入法状态
au BufEnter * let b:im=0
" 为了尽量不影响体验，通过异步机制来进行处理
" 下面的函数用于在 job 进行过程中处理返回信息
fun! IMHandler(channel, msg)
    if a:msg !=? 'com.apple.keylayout.ABC'
        " 切换到英文输入法之前保存当前状态
        let b:im = 1
        call job_start(['issw', 'com.apple.keylayout.ABC'])
    endif
endfun
fun! Lang2en()
    let job = job_start(['issw'], {"out_cb": "IMHandler"})
endfun
" 这里中文输入法的切换用了自己写的脚本，虽然也可以用上面的 issw 命令。
fun! Lang2zh()
    if b:im == 1
        call job_start(['switchim'])
    endif
endfun
autocmd InsertEnter * call Lang2zh()
autocmd BufEnter,InsertLeave * call Lang2en()
```

因为不想用破解软件来修改按键映射及快捷键，导致仍有不足之处：

- 调用的外部程序切换输入法不够快，仍然有一定几率产生困扰。
- insert 模式下，如果要用搜狗输入法，从英文切换回中文比较麻烦。
