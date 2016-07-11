#SingleInstance force
#NoEnv
#NoTrayIcon
SendMode Input
SetWorkingDir %A_ScriptDir%

commands=
(
net use X: [Your URL Destination Here] /persistent:yes
)
runwait, %comspec% /c %commands%,,Hide

set = 1
height := A_ScreenHeight-260
width := A_ScreenWidth-530
FormatTime, CurrentYear,, yyyy
data_dir=X:\ESPO\AllOrders\%CurrentYear%

Gui, +AlwaysOnTop
gui, font, bold,
Gui, add,edit, x10 y10 w500 h140  vinfo -vscroll +disabled,Send files directly to SharePoint Documents Related to Orders.`n`nHold Ctrl and begin selecting files you wish to move.`nDrag and drop the selected files into this box.`nPress Upload Files.
gui, font, normal,
Gui, add,button, w100 gdata,Upload Files
Gui, Add, Button, xp+105 w100 gClear, Clear / Info
Gui, show, x%width% y%height% w520 h185 ,Orders Uploader
SetTimer, MouseOverPicture ,1
return 

Clear:
reload
Return

data:
if (list="")
MsgBox, nofiles in list
_dir:=data_dir
gosub move_them
MsgBox, File(s) Uploaded Successfully!
reload
return


move_them:
Loop, parse, list, `n
{
splitpath,A_LoopField,name
FileCopy, %A_LoopField%,%_dir%
}
return 

guidropfiles:
GuiControl,, MyEdit
FileList = %A_GuiEvent%
Loop, parse, FileList, `n
list .=A_LoopField "`n"
GuiControl,, MyEdit, %list%
return 


MouseOverPicture:
Gui, Submit, NoHide
MouseGetPos,,,,id, 2
if ( id = Picture)
{
   if  ( set = "1" )
   {
GuiControl,hide, Info,
gui, font, bold,
Gui, add,edit, x10 y10 w500 h140  vmyedit
     set = 0
   }
   return
}
if ( set = "0" )
{
}
return


GuiClose:
ExitApp

