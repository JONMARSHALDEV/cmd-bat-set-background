# cmd-bat-set-background

it must be tested
```
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d path.bmp /f 
RUNDLL32.EXE user32.dll,UpdatePerUserSystemParameters 
pause
```
as:
```
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v Wallpaper /t REG_SZ /d e:/image.bmp /f 
```
see details:
https://www.quora.com/How-do-I-make-a-bat-file-to-change-the-Windows-background-to-a-picture-of-my-choosing

=====================

also try:
```
Group Policy
Run the Local Group Policy Editor (gpedit.msc) and position to Computer Configuration > Administrative Templates > Control Panel > Personalization. Set "Force a specific default lock screen and logon image" to Enabled, enter the path to the image, and click OK.

Registry
Use regedit to navigate to the key HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Personalization. If the subkey Personalization does not exist, then create it. Create a New String Value named LockScreenImage and set it to the full path of the image.
```
see details:
https://superuser.com/questions/1649955/how-do-i-change-the-login-screen-image-displayed-after-a-reboot-on-windows-10/1653140#1653140


or via powershell:
```
# Change this to the path where you keep the desired background image
$imagePath = 'C:\path\to\image.ext'

$newImagePath = [System.IO.Path]::GetDirectoryName($imagePath) + '\' + (New-Guid).Guid + [System.IO.Path]::GetExtension($imagePath)
Copy-Item $imagePath $newImagePath
[Windows.System.UserProfile.LockScreen,Windows.System.UserProfile,ContentType=WindowsRuntime] | Out-Null
Add-Type -AssemblyName System.Runtime.WindowsRuntime
$asTaskGeneric = ([System.WindowsRuntimeSystemExtensions].GetMethods() | ? { $_.Name -eq 'AsTask' -and $_.GetParameters().Count -eq 1 -and $_.GetParameters()[0].ParameterType.Name -eq 'IAsyncOperation`1' })[0]
Function Await($WinRtTask, $ResultType) {
    $asTask = $asTaskGeneric.MakeGenericMethod($ResultType)
    $netTask = $asTask.Invoke($null, @($WinRtTask))
    $netTask.Wait(-1) | Out-Null
    $netTask.Result
}
Function AwaitAction($WinRtAction) {
    $asTask = ([System.WindowsRuntimeSystemExtensions].GetMethods() | ? { $_.Name -eq 'AsTask' -and $_.GetParameters().Count -eq 1 -and !$_.IsGenericMethod })[0]
    $netTask = $asTask.Invoke($null, @($WinRtAction))
    $netTask.Wait(-1) | Out-Null
}
[Windows.Storage.StorageFile,Windows.Storage,ContentType=WindowsRuntime] | Out-Null
$image = Await ([Windows.Storage.StorageFile]::GetFileFromPathAsync($newImagePath)) ([Windows.Storage.StorageFile])
AwaitAction ([Windows.System.UserProfile.LockScreen]::SetImageFileAsync($image))
Remove-Item $newImagePath
```

and use it:
```
powershell -file .\lockscr.ps1
```
see details:
https://superuser.com/questions/1343623/how-do-i-change-my-lock-screen-picture-automatically/1343640#1343640
