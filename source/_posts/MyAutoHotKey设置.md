---
title: MyAutoHotKey设置
date: 2022-04-10 14:43:11
time: 14-43
categories:
- AHK 
tags:
- AHK

---

我的MyAutoHotKey设置,欢迎下载使用(后续尽量更新，请灵活使用) 

[CapsLock.exe](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/AHK/Capslock%20.exe)

[Space.exe](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/AHK/Space.exe)

[MyAHK.exe](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/AHK/MyAHK.exe)

功能如下：

<!--more-->

1. Space.ahk
   1. space + 数字 ……代替 shift + 数字一类
   2. space  + ikjl,h,n,g,b,c,v,x,z,y代替光标上下左右，行首，行尾，换行，删除,ctrl+c,v,x,z,y
   3. space + a + ?   为我大部分操作
   4. space + u + ?  为我部分操作
2. CapsLock.ahk   
   1.  CapsLock + ikjl,h,n,g,b光标移动
   2. CapsLock + wsad,q,e,r,f 模拟鼠标上下左右移动，点击，右键，向上滚动，向下滚动
   3. CapsLock + 数字  切换桌面
3. MyAHK.ahk
   1. 一些热键和热字符串
   2. 在任务栏上滚动滚轮:增加/减小音量.
   3. 待补充



编年史：

2022年4月10日22:26:17 

CapsLock.ahk增强鼠标丝滑度。来自https://www.autoahk.com/archives/4340。 (不愧是我，缝合怪)

# Space.ahk

{% spoiler "xlr-space.ahk" %}

```
; -------------------------------------空格快捷键
; ---------一些权限控制
;管理员运行
if not A_IsAdmin
{
   Run *RunAs "%A_ScriptFullPath%" 
   ExitApp
}

; MsgBox, 4,, 为以后的.ahk脚本添加以管理员权限运行吗？`n（选“否”可退回默认用户权限）
; IfMsgBox Yes
; {
; RegWrite, REG_SZ, HKCU\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers,%A_AhkPath%, ~ RUNASADMIN
; RegWrite, REG_SZ, HKCR\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers,%A_AhkPath%, ~ RUNASADMIN
; MsgBox 添加.ahk脚本管理员权限成功，`n以后的.ahk脚本默认都是以管理员权限运行。
; Return
; } else {
; RegDelete, HKCU\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers,%A_AhkPath%, ~ RUNASADMIN
; RegDelete, HKCR\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers,%A_AhkPath%, ~ RUNASADMIN
; MsgBox 清除.ahk脚本管理员权限，`n以后的.ahk脚本都是以用户权限运行。
; Return
; }

; 无环境变量  
#NoEnv

; 高进程
Process Priority,,High

; 在ahk中 ctrl 为^; win 为#；alt 为！ ；shift 为 + 

;---------------空格快捷键具体操作

;--------系统自带空格快捷键 
<!space::Send {alt}{shift} ;输入法中英文切换,挺方便的，
                           ;个人不喜欢左手小脚拇指按shift键
space::Send {space}


;!space::Send !+  ;
;#space::Send #{space}
;^#space::Send ^#{space}
;^!space::Send ^!{space}

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
space & [::Send {{}
space & ]::Send {}}
space & `;::Send {:}
space & '::Send {"}








;---space快捷键2
space & c:: Send ^c
space & x:: Send ^x
space & v:: Send ^v
space & z:: Send ^z
space & y:: Send ^y

;---space快捷键3
#if GetKeyState("space", "P")
  ;-----空格光标快捷键
  ;   -space+ijkl ,o,,h,n上下左右，上页，下页，行首，行尾，

  t::Send {Ctrl down}{Alt down}{Tab}{Alt up}{Ctrl up}

  i:: Send {up}
  j:: Send {left}
  k:: Send {down}
  l:: Send {right}
  !j:: Send {left}
  !k:: Send {down}
  !l:: Send {right}
  !o:: Send {Pgup}   ;上翻页
  ,:: Send {Pgdn}   ;下翻页
  h:: Send {home}
  n:: Send {end}
  b:: Send {Backspace}    ;删除
  g:: Enter         ;回车


  ;浏览器操作
  o::^o
  w::!+w   ;上个标签页
  s::!+s   ;下个标签页
  d::!+x   ;关闭标签页
  ;f::!+f   ;打开彩云小译
  e::!+e   ;打开Pinbox 收藏网页

  ; space + w + ? 文件夹快捷键
  w & m:: run, D:/Me



  q::^q    ;打开 Vimiumc 搜索框
  


  a::return
  f::return


  c:: Send ^c
  x:: Send ^x
  v:: Send ^v
  z:: Send ^z
  y:: Send ^y

  ;-  space+s+ijkl 代替shift+up，left，down，right
  s & i:: Send +{up}
  s & j:: Send +{left}
  s & k:: Send +{down}
  s & l:: Send +{right}

  ;-  space+c+ijkl 代替 ctrl+up，left，down，right
  c & i:: Send ^{up}
  c & j:: Send ^{left}
  c & k:: Send ^{down}
  c & l:: Send ^{right}

  ;-  space+f+ijkl 代替 ctrl+shift+up，left，down，right
  f & i:: Send ^+{up}
  f & j:: Send ^+{left}
  f & k:: Send ^+{down}  
  f & l:: Send ^+{right}



  ;---空格+a  为我大部分快捷操作
  ;---空格+a+字母 系统操作

  ;获取当前时间data:%A_YYYY%-%A_MM%-%A_DD% %A_Hour%:%A_Min%:%A_Sec%
  a & 9::send   %A_YYYY%-%A_MM%-%A_DD% %A_DDD% %A_Hour%:%A_Min%:%A_Sec%
  a & 0::send   %A_Hour%{:}%A_Min%{space}


  a & e::send #e  ;打开文件管理器
  a & d::send #d  ;回到桌面
  a & x:: Send !{F4} ;关闭程序
  ;---音量控制
  a & w:: Send {Volume_Up}
  a & s:: Send {Volume_Down} 

  ;---空格+a+字母(按起来方便点) 切换窗口 & 启动程序
  
  
  a & r::#1 ;vivaldi 浏览器 
  a & f::#2 ;我来
  a & v::#3 ;typorn 
  a & t::#4 ;javaidea
  a & g::+enter ;obs
  a & b::#5
  a & y::#6
  a & h::#7
  a & n::#8
  a & u::!u
  a & j::!j 


  ;Alt
  ;idea 快捷键
  alt & w::Send ^{F4}       ;关闭当前代码窗
  alt & j::Send !{left}     ;左边代码窗
  alt & l::Send !{right}    ;右边代码窗
  alt & u::Send !{insert}   ;Alt+insert
  alt & r::Send +{F10}      ;运行当前类代码
  /::Send ^+/    ;代码块注释Ctrl+Shift+/ 
  alt & =::Send !{F12}        ;打开终端

  ;Logseq
  alt & i::Send !{up}     ;左边代码窗
  alt & k::Send !{down}    ;右边代码窗

  ;------------------------------
  ;---空格+u+? 小部分快捷操作
  ;space + u + 数字 代替F1...
  u::!u
  u & 1:: Send {F1}
  u & 2:: Send {F2}
  u & 3:: Send {F3}
  u & 5:: Send {F5}
  u & 9:: Send {F9}
  u & 0:: Send {F12}
  
  ;------空格+t+? 文本快捷键
  ;---删除一行 
  p & d::Send {Home}{ShiftDown}{End}{Right}{ShiftUp}{Del} 


  ;---复制整行
  p & g::
    Send {Home}{ShiftDown}{End}{Right}{ShiftUp}
    SendInput, ^c
    Send {Home}
  Return

  #if 多任务窗口切换界面内()
      d:: SendEvent {Blind}{Delete}{Right} ; 关闭应用 
    return
    多任务窗口切换界面内()
    {
        Return AltTabWindowGet() || WinTabWindowGet()
    }
    AltTabWindowGet()
    {
        return WinActive("ahk_class MultitaskingViewFrame") || WinActive("ahk_class XamlExplorerHostIslandWindow")
    }
    WinTabWindowGet()
    {
        return WinActive("ahk_class Windows.UI.Core.CoreWindow ahk_exe explorer.exe") || WinActive("ahk_class XamlExplorerHostIslandWindow")
    }
  return  

Return


```

{% endspoiler %}

# Capslock .ahk

{% spoiler "Capslock .ahk" %}

```
;管理员运行
; if not A_IsAdmin
; {
;    Run *RunAs "%A_ScriptFullPath%" 
;    ExitApp
; }

; ;无环境变量
; #NoEnv

; ;高进程
; Process Priority,,High

;一直关闭 Capslock
SetCapsLockState, AlwaysOff  

; CapsLock -> Esc
CapsLock::Send {Esc}
return
!CapsLock:: Send CapsLock

#if GetKeyState("CapsLock", "P")
x:: Send ^w ; 关闭标签
v:: 当前窗口临时透明()
v Up:: 当前窗口临时透明取消()
return

当前窗口临时透明()
{
    WinSet, Transparent, 100, A
    WinSet, Alwaysontop, On, A
}
当前窗口临时透明取消()
{
    WinSet, Transparent, 255, A
    WinSet, Alwaysontop, Off, A
}

; 光标移动&&&

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
If GetKeyState("shift")
MoveActiveWindowToDesktop(1) 
else SwitchToDesktop(1)
Return
Capslock & 2:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(2) 
else SwitchToDesktop(2)
Return
Capslock & 3:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(3) 
else SwitchToDesktop(3)
Return
Capslock & 4:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(4) 
else SwitchToDesktop(4)
Return
Capslock & 5:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(5) 
else SwitchToDesktop(5)
Return
Capslock & 6:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(6) 
else SwitchToDesktop(6)
Return
Capslock & 7:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(7) 
else SwitchToDesktop(7)
Return
Capslock & 8:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(8) 
else SwitchToDesktop(8)
Return
Capslock & 9:: 
If GetKeyState("shift")
MoveActiveWindowToDesktop(9) 
else SwitchToDesktop(9)
Return
Capslock & 0:: 
If GetKeyState("shift")
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

;-------------------------模拟鼠标增强
!CapsLock:: CapsLock
#If GetKeyState("CapsLock", "P")


a:: ta := (ta ? ta : QPC()), mTick()
d:: td := (td ? td : QPC()), mTick()
w:: tw := (tw ? tw : QPC()), mTick()
s:: ts := (ts ? ts : QPC()), mTick()
	a Up:: ta := 0, mTick()
	d Up:: td := 0, mTick()
	w Up:: tw := 0, mTick()
	s Up:: ts := 0, mTick()

e:: RButton
q:: LButton

r:: tr := (tr ? tr : QPC()), sTicky()
f:: tf := (tf ? tf : QPC()), sTicky()
r Up:: tr := 0, sTicky()
f Up:: tf := 0, sTicky()



CoordMode, Mouse, Screen

;-----------鼠标模式选择
ma(t){
	 Return ma2(t) ; 二次函数运动模型
	; Return ma3(t) ; 三次函数运动模型
	; 指数函数运动模型
	;return maPower(t)
}
ma2(t){
	; x-t 二次曲线加速运动模型
	; 跟现实世界的运动一个感觉
	If(0 == t)
	return 0
	If(t > 0)
	return  6
	else
		return -6

}

ma3(t){
	; x-t 三次曲线函数运动模型
	; 与现实世界不同，
	; 这个模型会让人感觉鼠标比较“重”
	;
	If(0 == t)
	return 0
	If(t > 0)
	return t * 12
	else
		return t * 12
}

maPower(t){
	; x-t 指数曲线运动的简化模型
	; 这个模型可以满足精确定位需求，也不会感到鼠标“重”
	; 但是因为跟现实世界的运动曲线不一样，凭直觉比较难判断落点，需要一定练习才能掌握。
	;
	If(0 == t)
	return 0
	If(t > 0)
	return  ( Exp( t) - 0.95 ) * 16
	else
		return -( Exp(-t) - 0.95 ) * 16
}

QPF()
{
	DllCall("QueryPerformanceFrequency", "Int64*", QuadPart)
	return QuadPart
}

QPC()
{
	DllCall("QueryPerformanceCounter", "Int64*", Counter)
	return Counter
}

; 时间计算
dt(t, tNow){
	return t ? (tNow - t) / QPF() : 0
}


MoCaLi(v, a){ ; 摩擦力
	If((a > 0 And v > 0) Or (a < 0 And v < 0))
	return v
	; 简单粗暴倍数降速
	v *= 0.8
	If(v > 0)
	v -= 1
	If(v < 0)
	v += 1
	v //= 1
	return v
}


; 鼠标加速度微分对称模型，每秒误差 2.5ms 以内
global ta := 0, td := 0, tw := 0, ts := 0, mvx := 0, mvy := 0

; 滚轮加速度微分对称模型（不要在意名字hhhh
global tr := 0, tf := 0, tz := 0, tc := 0, svx := 0, svy := 0

; 鼠标运动处理
mm:
	tNow := QPC()
	; 计算用户操作时间
	tda := dt(ta, tNow)
	tdd := dt(td, tNow)
	tdw := dt(tw, tNow)
	tds := dt(ts, tNow)

	; 计算加速度
	max := ma(tdd - tda)
	may := ma(tds - tdw)

	; 摩擦力不阻碍用户意志
	mvx := MoCaLi(mvx + max, max)
	mvy := MoCaLi(mvy + may, may)

	MouseMove, %mvx%, %mvy%, 0, R

	If(0 == mvx And 0 == mvy)
	SetTimer, mm, Off
return

; 时间处理
mTick(){
	SetTimer, mm, 1
}
mouseWheel_lParam(x, y){
	return x | (y << 16)
}

; 滚轮运动处理
msx:
	tNow := QPC()
	; 计算用户操作时间
	tdz := dt(tz, tNow)
	tdc := dt(tc, tNow)
	; 计算加速度
	sax := ma(tdc - tdz)
	svx := MoCaLi(svx + sax, sax)

	MouseGetPos, mouseX, mouseY, wid, fcontrol
	wParam := svx << 16 ;zDelta
	lParam := mouseWheel_lParam(mouseX, mouseY)
	PostMessage, 0x20E, %wParam%, %lParam%, %fcontrol%, ahk_id %wid%

	If(0 == svx)
	SetTimer, msx, Off
return
msy:
	tNow := QPC()
	; 计算用户操作时间
	tdr := dt(tr, tNow)
	tdf := dt(tf, tNow)
	; 计算加速度
	say := ma(tdr - tdf)
	svy := MoCaLi(svy + say, say)

	MouseGetPos, mouseX, mouseY, id, fcontrol
	wParam := svy << 16 ;zDelta
	lParam := mouseWheel_lParam(mouseX, mouseY)
	PostMessage, 0x20A, %wParam%, %lParam%, %fcontrol%, ahk_id %id%

	If(0 == svy)
	SetTimer, msy, Off
return

; 时间处理
sTickx(){
	SetTimer, msx, 1
}
sTicky(){
	SetTimer, msy, 1
}










```

{% endspoiler %}

# MyAHK

{% spoiler "MyAHK.ahk" %}

```

;----------------------------热键与字符串
;------字符串
;-其他
;-账号相关
::196::196156709
::196q::196156709@qq.com
::283::2833613437
::183::18370527825
::3622::362232200201160417
::123a::12345678a
::jts::今天是王冠荆棘自学Java的第61天
:*:hexoc::hexo clean && hexo g && start http://localhost:45678 && hexo s -p 45678 ;hexo快捷输入
^!x::^1 


;------热键     {Alt down}{Tab}{Alt up}  
;ctrl
; #if GetKeyState("ctrl", "P")
;     s::#1
;     d::#2
;     f::#3

; return

;--------------------------音量快捷键

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

{% endspoiler %}

