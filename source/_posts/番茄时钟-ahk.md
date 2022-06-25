---
title: 番茄时钟.ahk
time: 17-59
date: 2022-04-12 17:59:16
categories:
- AHK
tags:
- AHK


---

1. 番茄时钟.ahk

①25 分钟固定循环休息提醒。 （每小时的 25 分和 55 分播放休息铃声，00 分和 30 分播放工作铃声，） 

②使用番茄时钟时，到点自动切换桌面。（工作桌面为 1，休息桌面为 5） 

* 铃声可以选自己喜欢的 
* 时间可以自己设置 
* 切换桌面自己设置 
* 增加了提示信息，
*  (我是 切换桌面 提示信息 停顿5秒 播放音乐，）
* 2022年4月12日14:14:38
* 2022年4月19日15:02:19

<!--more-->

2.简单计时器   

``` 
#t:: 
InputBox,time,计时器,请输入一个时间(以秒) 
time := time* 1000 
Sleep, %time% 
MsgBox 时间到了 
SoundPlay % "C:\Windows\media\Ring01.wav" 
return
```

番茄时钟.ahk

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/FpAS-smNPflQSZff2UB6Jl5K9gQ7.jpg)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650352005636.png)

{% spoiler "番茄时钟.ahk" %}

``` 
  ; ========== CapsLockX ==========
; 名称：番茄时钟
; 描述：番茄时钟
; 作者：snomiao
; 联系：snomiao@gmail.com
; 支持：https://github.com/snomiao/CapsLockX
; 版本：v2021.03.26
; ========== CapsLockX ==========

global T_TomatoLife := ("TomatoLife", "Enable", 1, "使用番茄时钟（默认禁用，改为 1 开启）")
global T_TomatoLife_NoticeOnLaunch := ("TomatoLife", "NoticeOnLaunch", 1, "启动时报告番茄状态")
global T_TomatoLife_UseTomatoLife := ("TomatoLife", "UseTomatoLife", 1, "使用番茄报时（00分和30分播放工作铃声，每小时的25分和55分播放休息铃声）（需要先开启番茄时钟）")
global T_TomatoLife_UseTomatoLifeSwitchVirtualDesktop := ("TomatoLife","UseTomatoLifeSwitchVirtualDesktop", 1, "使用番茄报时时，自动切换桌面（休息桌面为5，工作桌面为1）")

if (T_TomatoLife) {
    高精度时间配置()
    GoSub 定时任务
    ; [有一个难以复现的 bug・Issue #17・snolab/CapsLockX]( https://github.com/snolab/CapsLockX/issues/17 )
}

Return

高精度时间配置(){
    ; global T_TomatoLife := CapsLockX_Config("TomatoLife", "", 0, "使用定时任务")
    ; MsgBox, 你开启了定时任务，是否现在配置高精度时间？
    ; IfMsgBox, Cancel
    ; return
    
    global T_TomatoLife_UsingHighPerformanceTime := ("TomatoLife", "T_UsingHighPerformanceTime", "0", "已经配置过高精度时间的Flag")
    if (T_TomatoLife_UsingHighPerformanceTime)
        return
    ToolTip, 定时任务开启，正在为您配置系统高精度时间
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" /v "FrequencyCorrectRate" /t REG_DWORD /d 2 /f, , Hide
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" /v "UpdateInterval" /t REG_DWORD /d 100 /f, , Hide
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" /v "MaxPollInterval" /t REG_DWORD /d 6 /f, , Hide
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" /v "MinPollInterval" /t REG_DWORD /d 6 /f, , Hide
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" /v "MaxAllowedPhaseOffset" /t REG_DWORD /d 0 /f, , Hide
    RunWait reg add "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient" /v "SpecialPollInterval" /t REG_DWORD /d 64 /f, , Hide
    RunWait net stop w32time, , Hide
    RunWait net start w32time, , Hide
    ("TomatoLife", "T_UsingHighPerformanceTime", "1", "")
    ToolTip
}
番茄状态计算(){    ;这里修改时间
    Return ((Mod((UnixTimeGet() / 60000)+30000, 30) < 25) ? "工作时间" : "休息时间")
}

番茄报时(force:=0){
    ; 检测睡眠标记文件以跳过报时
    static SLEEPING_FLAG_CLEAN := 0
    if(!SLEEPING_FLAG_CLEAN) {
        ; 启动时重置标记文件
        FileDelete %TEMP%/SLEEPING_FLAG
        SLEEPING_FLAG_CLEAN := 1
    } else {
        FileRead SLEEPING_FLAG, %TEMP%/SLEEPING_FLAG
        if (SLEEPING_FLAG) {
            Return
        }
    }
    番茄状态 := 番茄状态计算()
    ; 边沿触发过滤器
    
    ; static 上次番茄状态 := ""
    static 上次番茄状态 := 番茄状态计算()
    
    ; msgbox %上次番茄状态% %番茄状态%
    if (上次番茄状态 == 番茄状态 && !force) {
        Return
    }
    上次番茄状态 := 番茄状态
    ; MsgBox, 番茄：%番茄状态%
    ; TrayTip, 番茄：%番茄状态%, ： %番茄状态%
    ; 状态动作
    if ("工作时间" == 番茄状态) {
        Send {space}  ;如果在看视频时，先暂停；
        
        ;  SoundPlay % "C:\Windows\media\Alarm01.wav"   时间提醒,,这里可以切换铃声
        ;   ; 暂缓3秒
        SwitchToDesktop(1)
	MsgBox 学习时间到了      ;提示框    分号是注释
	sleep 5000 ; 暂缓3秒
	SoundPlay % "C:\Windows\media\倒带蔡依林.mp3" ; 
        
        ; Drive MsgBox,4100
        
        ; MsgBox内容的5秒倒计时
        ; Secs := 5
        ; SetTimer, CountDown, 1000
        ; MsgBox, 1, 番茄时钟, 需要 休息吗 %Secs% 秒？, %Secs%
        ; SetTimer, CountDown, Off
        ; IfMsgBox Ok
        ; SwitchToDesktop(5)
        ; Return

        ; CountDown:
        ; Secs -= 1
        ; ControlSetText,Static1,需要 休息吗 %Secs% 秒？,番茄时钟 ahk_class #32770
        ; Return
        
    }
    if ("休息时间" == 番茄状态) {
        Send {space}  ;如果在看视频时，先暂停；
        
        
        ; SoundPlay % "C:\Windows\media\Ring01.wav"        
        SwitchToDesktop(5)
	MsgBox 休息时间到了
	sleep 5000 ; 暂缓3秒
	SoundPlay % "C:\Windows\media\口琴.mp3" ; 时间提醒,,这里可以切换铃声
        
        
        ;MsgBox内容的20秒倒计时
        ; Secs := 5
        ; SetTimer, CountDown2, 1000
        ; MsgBox, 1, 番茄时钟, 需要 休息吗 %Secs% 秒？, %Secs%
        ; SetTimer, CountDown, Off
        ; IfMsgBox Ok
        ; SwitchToDesktop(1)
        ; Return

        ; CountDown2 :
        ; Secs -= 1
        ; ControlSetText,Static1,需要 休息吗 %Secs% 秒？,番茄时钟 ahk_class #32770
        ; Return
    }
}

UnixTimeGet()
{
    ; ref: https://www.autohotkey.com/boards/viewtopic.php?t=17333
    t := A_NowUTC
    EnvSub, t, 19700101000000, Seconds
    Return t * 1000 + A_MSec
}

定时任务:
    if (T_TomatoLife_UseTomatoLife)
        番茄报时()
    间隔 := 60000 ; 间隔为1分钟，精度到毫秒级
    延时 := (间隔 - Mod(UnixTimeGet(), 间隔))
    ; ToolTip, % 延时
    SetTimer 定时任务, %延时%
Return

#If
^!i::
    ; 番茄状态 := 番茄状态计算()
    ; MsgBox, 番茄状态：%番茄状态%
    番茄报时()
return

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

{% endspoiler %}

