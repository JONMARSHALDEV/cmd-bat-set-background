# cmd-bat-set-background

it must be tested

reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d path.bmp /f 
RUNDLL32.EXE user32.dll,UpdatePerUserSystemParameters 
pause

as:

reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d e:/image.bmp /f 

see details:
https://www.quora.com/How-do-I-make-a-bat-file-to-change-the-Windows-background-to-a-picture-of-my-choosing

=====================

also try:
Group Policy
Run the Local Group Policy Editor (gpedit.msc) and position to Computer Configuration > Administrative Templates > Control Panel > Personalization. Set "Force a specific default lock screen and logon image" to Enabled, enter the path to the image, and click OK.

Registry
Use regedit to navigate to the key HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Personalization. If the subkey Personalization does not exist, then create it. Create a New String Value named LockScreenImage and set it to the full path of the image.

see details:
https://superuser.com/questions/1649955/how-do-i-change-the-login-screen-image-displayed-after-a-reboot-on-windows-10/1653140#1653140
