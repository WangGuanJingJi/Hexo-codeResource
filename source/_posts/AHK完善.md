---
title: AHK完善
date: 2022-04-10 01:55:42
time: 01-55
categories:
- AHK 
tags:
- AHK

---

介绍一款基于 AutoHotkey 的创建的CapsLockX  ，挺好用的，但与我的AHK脚本冲突了（MyAHKahk，xlr-space.ahk）

这三个得想办法不冲突，运行。

<!--more-->



# 介绍CapsLockX 

# [CapsLockX – 像黑客一样操作电脑！](https://www.autoahk.com/archives/34996)

CapsLockX 是一款基于 AutoHotkey 的模块化热键脚本引擎。 让你可以轻轻松松像电影里的**黑客**一样，双手不离开键盘，**高效率**地操作电脑。这里有超多一摸就懂超好上手的功能：编辑增强、虚拟桌面与窗口管理、鼠标模拟、应用内热键增强、JS 数学表达式计算、等超多功能等你来亲自定义。主仓库地址 🏠：[https://github.com/snolab/CapsLockX](https://github.com/snolab/CapsLockX)。非常牛。

# AHK-完善两种方案

## 一改变CapsLockX

取消CapsLockX 使用空格键，CapsLockX 用CapsLook 键已经足够了，这是最简单的方式。这样就不会与xlr-space.ahk（空格脚本冲突了）

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/Snipaste_2022-04-10_01-45-29.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/Snipaste_2022-04-10_01-46-39.png)



; 在这里你也可以定义无需按下 CapsLockX 就能触发的热键&&&

在这后面添加自己的ahk.

```text
; 在这里你也可以定义无需按下 CapsLockX 就能触发的热键&&&

;----------------------------热键与字符串

;------热键
;---加强复制，复制后返回 win2
^+c:: 
  Send,^c
  Send,#2
return

;---字符串
;-账号相关
::196::196156709
::196q::196156709@qq.com

;-其他
:*:hexoc::
   Send, hexo clean && hexo g && hexo s ;hexo快捷输入
Return

;-------------------------------------空格快捷键

;---输入一个space打印一个space,及相关系统 space键
space::Send {space}
!space::Send !{space}
^space::Send ^{space}
#space::Send #{space}
^#space::Send ^#{space}
^!space::Send ^!{space}

;--- space + Num 代替 shift+Num
space & 1::Send {!}
space & 2::Send {@}
space & 3::Send {#}
space & 4::Send {$}
space & 5::Send {`%}
space & 6::Send {^}
space & 7::Send {&}
space & 8::Send {*}
space & 9::Send {(}
space & 0::Send {)}
space & -::Send {_}
space & =::Send {+}

;---space快捷键2
space & c:: Send ^c
space & x:: Send ^x
space & v:: Send ^v
space & z:: Send ^z
space & y:: Send ^y
   
;---space光标快捷键
#if GetKeyState("space", "P")

  ;   -space+ijkl ,o,,h,n上下左右，上页，下页，行首，行尾，
  i:: Send {up}
  j:: Send {left}
  k:: Send {down}
  l:: Send {right}
  o:: Send {Pgup}
  ,:: Send {Pgdn}
  h:: Send {home}
  n:: Send {end}{Enter}   ;space+n 去到最后一行并换行
  b:: Send {Blind}{Delete}
  g:: Enter

  ;-  space+s+ijkl 代替shift+up，left，down，right
  +i:: Send +{up}
  +j:: Send +{left}
  +k:: Send +{down}
  +l:: Send +{right}

  ;-  space+c+ijkl 代替 ctrl+up，left，down，right
  ^i:: Send ^{up}
  ^j:: Send ^{left}
  ^k:: Send ^{down}
  ^l:: Send ^{right}

  ;-  space+f+ijkl 代替 ctrl+shift+up，left，down，right
  ^+i:: Send ^+{up}
  ^+j:: Send ^+{left}
  ^+k:: Send ^+{down}  
  ^+l:: Send ^+{right}

  ;------文本空格快捷键
  ;---删除一行
  t & d::Send {Home}{ShiftDown}{End}{Right}{ShiftUp}{Del} 

  ;---复制整行
  t & g::
    Send {Home}{ShiftDown}{End}{Right}{ShiftUp}
    SendInput, ^c
    Send {Home}
  Return

  ;---空格+u+数字 切换窗口
  u & 1::#1
  u & 2::#2
  u & 3::#3
  u & 4::#4
  u & 5::#5

  ;---空格+a+字母 启动程序
  a & j::run Notepada
  a & 1::run C:\Program Files\Google\Chrome\Application\chrome.exe
  a & 2::run D:\APP\Tools\Text tool\wolai\wolai.exe
  a & 3::run D:\APP\Tools\Other tool\Clash\Clash for Windows\Clash for Windows.exe 
  a & 4::run C:\Drcom\DrUpdateClient\DrMain.exe  
Return



;--------------------------音量快捷键

;---音量键①
+NumpadAdd:: Send {Volume_Up}
+NumpadSub:: Send {Volume_Down}
break::Send {Volume_Mute}
return

; 在任务栏上滚动滚轮:增加/减小音量.②
#If MouseIsOver("ahk_class Shell_TrayWnd")
WheelUp::Send {Volume_Up}     ; 在任务栏上滚动滚轮:增加/减小音量.
WheelDown::Send {Volume_Down} ;
MouseIsOver(WinTitle) {
    MouseGetPos,,, Win
    return WinExist(WinTitle . " ahk_id " . Win)
}
Return
```

OK，有些小问题，但不弄了。

## 二在我的基础上再创建一个CapsLook.ahk脚本

这样感觉好点，自己弄的，功能都清楚。

```text
;管理员运行
if not A_IsAdmin
{
   Run *RunAs "%A_ScriptFullPath%" 
   ExitApp
}

;无环境变量
#NoEnv

;高进程
Process Priority,,High

;一直关闭 Capslock
SetCapsLockState, AlwaysOff  

; CapsLock -> Esc
CapsLock::
Send {Esc}
return

CapsLock & space:: Send "#{t}"           ;触发 win+t

; 光标移动
CapsLock & a::
MouseMove, -15, 0, 0, R                                               
return  
CapsLock & s::                                                       
MouseMove, 0, 15, 0, R                                                
return                                                               
CapsLock & w::                                                       
MouseMove, 0, -15, 0, R                                                  
return                                                               
CapsLock & d::                                                       
MouseMove, 15, 0, 0, R                                              
return 

; 左键单击 
CapsLock & q::                                                       
SendEvent {Blind}{LButton down}                                      
KeyWait Enter                                                        
SendEvent {Blind}{LButton up}                                                
return 

; 右键单击 
CapsLock & e::                                                       
SendEvent {Blind}{RButton down}                                      
KeyWait Enter                                                        
SendEvent {Blind}{RButton up}                                                
return

; r 向上滚动
CapsLock & r:: 
SendEvent {Blind}{WheelUp}
return  

; 分号 向下滚动
CapsLock & f::
SendEvent {Blind}{WheelDown}
return 


; 指针移动
CapsLock & i::
Send {Up}
return
CapsLock & k::
Send {Down}
return
CapsLock & j::
Send {Left}
return
CapsLock & l::
    Send {right}
return

; 行首行尾
CapsLock & h::
Send {home}
return
CapsLock & n::
Send {end}{enter}
return


; 左右删除
CapsLock & b::
Send {BS}
return


; 撤销重做
CapsLock & z::
Send ^{z}
return


; Capslock + 数字  -->  切换桌面
; Capslock + Shift + 数字  -->  把当前窗口带到某桌面
; [Switch to desktop] OR [Move the current window to the X-th desktop]

Capslock & 1:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(1) 
else SwitchToDesktop(1)
Return
Capslock & 2:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(2) 
else SwitchToDesktop(2)
Return
Capslock & 3:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(3) 
else SwitchToDesktop(3)
Return
Capslock & 4:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(4) 
else SwitchToDesktop(4)
Return
Capslock & 5:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(5) 
else SwitchToDesktop(5)
Return
Capslock & 6:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(6) 
else SwitchToDesktop(6)
Return
Capslock & 7:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(7) 
else SwitchToDesktop(7)
Return
Capslock & 8:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(8) 
else SwitchToDesktop(8)
Return
Capslock & 9:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(9) 
else SwitchToDesktop(9)
Return
Capslock & 0:: 
If GetKeyState("Shift")
MoveActiveWindowToDesktop(10) 
else SwitchToDesktop(1)
Return
; Capslock & -:: 
;If GetKeyState("Shift")
;MoveActiveWindowToDesktop(11) 
;else SwitchToDesktop(1)
;Return
; Capslock & =:: 
;If GetKeyState("Shift")
;MoveActiveWindowToDesktop(12) 
;else SwitchToDesktop(1)
;Return
 
SwitchToDesktop(idx){
    if (!SwitchToDesktopByInternalAPI(idx)){
        TrayTip , WARN, SwitchToDesktopByHotkey
        SwitchToDesktopByHotkey(idx)
    }
}
SwitchToDesktopByHotkey(idx){
    SendInput ^#{Left 10}
    idx -= 1
    Loop %idx% {
        SendInput ^#{Right}
    }
}
 
SwitchToDesktopByInternalAPI(idx){
    succ := 0
    IServiceProvider := ComObjCreate("{C2F03A33-21F5-47FA-B4BB-156362A2F239}", "{6D5140C1-7436-11CE-8034-00AA006009FA}")
    IVirtualDesktopManagerInternal := ComObjQuery(IServiceProvider, "{C5E0CDCA-7B6E-41B2-9FC4-D93975CC467B}", "{F31574D6-B682-4CDC-BD56-1827860ABEC6}")
    ObjRelease(IServiceProvider)
    if (IVirtualDesktopManagerInternal){
        GetCount := vtable(IVirtualDesktopManagerInternal, 3)
        GetDesktops := vtable(IVirtualDesktopManagerInternal, 7)
        SwitchDesktop := vtable(IVirtualDesktopManagerInternal, 9)
        ; TrayTip, , % IVirtualDesktopManagerInternal
        pDesktopIObjectArray := 0
        DllCall(GetDesktops, "Ptr", IVirtualDesktopManagerInternal, "Ptr*", pDesktopIObjectArray)
        if (pDesktopIObjectArray){
            GetDesktopCount := vtable(pDesktopIObjectArray, 3)
            GetDesktopAt := vtable(pDesktopIObjectArray, 4)
            DllCall(GetDesktopCount, "Ptr", IVirtualDesktopManagerInternal, "UInt*", DesktopCount)
            ; if idx-th desktop doesn't exists then create a new desktop
            if (idx > DesktopCount){
                diff := idx - DesktopCount
                loop %diff% {
                    Send ^#d
                }
                succ := 1
            }
            GetGUIDFromString(IID_IVirtualDesktop, "{FF72FFDD-BE7E-43FC-9C03-AD81681E88E4}")
            DllCall(GetDesktopAt, "Ptr", pDesktopIObjectArray, "UInt", idx - 1, "Ptr", &IID_IVirtualDesktop, "Ptr*", VirtualDesktop)
            ObjRelease(pDesktopIObjectArray)
            if (VirtualDesktop){
                DllCall(SwitchDesktop, "Ptr", IVirtualDesktopManagerInternal, "Ptr", VirtualDesktop)
                ObjRelease(VirtualDesktop)
                succ := 1
            }
        }
        ObjRelease(IVirtualDesktopManagerInternal)
    }
    Return succ
}
vtable(ptr, n){
    ; NumGet(ptr+0) Returns the address of the object's virtual function
    ; table (vtable for short). The remainder of the expression retrieves
    ; the address of the nth function's address from the vtable.
    Return NumGet(NumGet(ptr+0), n*A_PtrSize)
}
GetGUIDFromString(ByRef GUID, sGUID) ; Converts a string to a binary GUID
{
    VarSetCapacity(GUID, 16, 0)
    DllCall("ole32\CLSIDFromString", "Str", sGUID, "Ptr", &GUID)
}
 
MoveActiveWindowToDesktop(idx){
    activeWin := WinActive("A")
    WinHide ahk_id %activeWin%
    SwitchToDesktop(idx)
    WinShow ahk_id %activeWin%
    WinActivate ahk_id %activeWin%
}
```
