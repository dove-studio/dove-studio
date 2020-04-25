---
title: 如何删除Outlook中的重复邮件
categories:
  - Windows
tags:
  - Outlook
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: c64db8d0
date: 2020-04-05 00:06:00
---
### 问题背景：

outlook 卸载重装后，会把之前已收的邮件，再次下载到本地，出现大量重复邮件。

### 解决思路：

搜索outlook邮件删除重复邮件的工具，有outlook duplicate items remover，Duplicate Email Remover，NoMoreDupes for Outlook等。但这些工具都要收费。故换了个思路，用宏来删除。

### 使用要点：

打开outlook，按快捷键Alt+F11，建立工程，并复制宏。
```
Option Explicit
 
Sub DeleteDuplicateEmailsInSelectedFolder()
 
Dim i As Long
Dim n As Long
Dim DeletedCount As Long
Dim Message As String
Dim Items As Object
Dim AppOL As Object
Dim NS As Object
Dim Folder As Object
 
Set Items = CreateObject("Scripting.Dictionary")
 
'Initialize and instance of Outlook
Set AppOL = CreateObject("Outlook.Application")
 
'Get the MAPI Name Space
Set NS = AppOL.GetNamespace("MAPI")
 
'Allow the user to select a folder in Outlook
Set Folder = NS.PickFolder
 
'Get the count of the number of emails in the folder
n = Folder.Items.Count
 
'Set the initial deleted count
DeletedCount = 0
 
'Check each email starting from the last and working backwards to 1
'Loop backwards to ensure that the deleting of the emails does not interfere with subsequent items in the loop
For i = n To 1 Step -1
 
    On Error Resume Next
    'Load the matching criteria to a variable
    'This is setup to use the Sunject and Body, additional criteria could be added if desired
    Message = Folder.Items(i).Subject & "|" & Folder.Items(i).Body
 
        'Check a dictionary variable for a match
        If Items.Exists(Message) = True Then
        'If the item has previously been added then delete this duplicate
        Folder.Items(i).Delete
        DeletedCount = DeletedCount + 1
    Else
        'In the item has not been added then add it now so subsequent matches will be deleted
        Items.Add Message, True
End If
 
Next i
 
ExitSub:
 
'Release the object variables from memory
Set Folder = Nothing
Set NS = Nothing
Set AppOL = Nothing
 
MsgBox "共删除" & DeletedCount & "封邮件。"
 
End Sub
```

然后F5运行此宏即可，如果提示宏被禁用，主菜单中文件->选项->信任中心，信任中心设置->宏设置，选择“启动所有宏”或者“为所有宏提供通知”。亲测Outlook2013有效，运行时需耐心等待。