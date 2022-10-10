# cmd-bat-set-background

it must be tested

reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d path.bmp /f 
RUNDLL32.EXE user32.dll,UpdatePerUserSystemParameters 
pause

as:

reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d e:/image.bmp /f 

see details:
https://www.quora.com/How-do-I-make-a-bat-file-to-change-the-Windows-background-to-a-picture-of-my-choosing
