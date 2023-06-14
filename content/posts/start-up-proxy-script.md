---
title: "Win10 Startup Proxy Script"
date: 2023-06-14T02:34:03-04:00
draft: false
---

公司电脑启动后需要手动设置proxy才能访问一些网站，每次手动设置有一些浪费时间，于是想到写一个开机运行的自动脚本。

```
@echo off
rem Set the PAC script URL

set newURL= "http://www.example.com/example.pac"

reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /v AutoConfigURL /t REG_SZ /d %newURL% /f

echo AutoConfigURL has been update to %newURL%.

pause
```
To access startup folder in win10, press Windows+R and enter "shell:startup"  
更改扩展为.bat文件然后放到上述文件夹。和开机时设置proxy script address为http://www.example.com/example.pac等效。

按照惯例让GPT解释一下代码  
>The code starts with "@echo off" which turns off the displaying of commands while they are executed.  
The code then sets a variable named "newURL" to the desired PAC script URL. In this case, it is set to "example.com/example.pac".  
The next line uses the "reg add" command to add or modify a registry key value. The key being modified is "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" and the entry being modified is "AutoConfigURL". This is where the new PAC script URL is set.  
The "/t REG_SZ" specifies the registry value type as a string type and the "/d %newURL%" specifies the new value as the value of the "newURL" variable created earlier. The "/f" flag is used to force the command to overwrite any existing values.  
Finally, the code prints a message saying that the AutoConfigURL has been updated to the new URL. The "pause" command causes the script to pause until the user presses a key, allowing the user to read the message before the program terminates.


