---
title: PDF复制粘贴去除多余的回车符
date: 2017-07-29 23:03:08
categories:
- Tools
tags:
- Tools
- Autohotkey
---

直接上解决步骤，但是只能适用于Windows平台，Mac这边可以尝试用[Alfred](https://www.alfredapp.com/) + workflow来对剪切板操作来解决，或者用[BetterTouchTool](https://www.boastr.net/)的自带个性化功能来尝试。只是一个思路，没有在Mac系统尝试

<!-- more --> 

- 下载 [Autohotkey](https://autohotkey.com/download/) ，安装（这一步都卡住那估计救不了了）
- 桌面右键 ➜ 新建 ➜ 创建新的AutoHotkey Script
- 右键创建的文件 ➜ 选择 Edit Script 出来一个记事本
- 编辑记事本文件，在已经有的内容下直接加上

```
#IfWinActive ahk_class classFoxitReader
^c:: 
    old := ClipboardAll
    clipboard := ""
    send ^c
    clipwait 0.1
    if clipboard = 
        clipboard := old
    else {
        tmp := RegExReplace(clipboard, "(\S.*?)\R(.*?\S)", "$1 $2")
        clipboard := tmp
        StringReplace clipboard, clipboard, % "  ", % " ", A
        clipwait 0.1
        }
    old := ""
    tmp := ""
return
```

> 这里有个问题 `IfWinActive ahk_class classFoxitReader` 第一行的`classFoxitReader` 是指的你用什么程序打开PDF 
>
> 如果是FoxitReader就是`classFoxitReader ` 如果是Acrobat Adobe就是`AcrobatSDIWindow`
>
> 可以用Autohotkey中的  [WinGetClass](https://autohotkey.com/docs/commands/WinGetClass.htm) 来获得某一个窗口的`ahk_class`

- 保存退出
- 桌面上双击你刚刚编辑的文件，可以看到右下角出现了一个`H`形状的图标

大功告成，这时候你再试试去PDF文档里面`ctrl + c`就没有回车符了（当然，段落还是无法区分的），也不一定，这一段既然是脚本语言，那就有无限的可能性，就看你的算法实现能力了对吧！