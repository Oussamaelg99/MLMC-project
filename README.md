@echo off
REM Open VS Code and navigate to the repository in the terminal
cd /d "C:\Path\To\Your\Repository"
start "" "C:\Path\To\VSCode\Code.exe" .
start "" "C:\Path\To\VSCode\Code.exe"

REM Open Outlook
start "" "C:\Path\To\Outlook\OUTLOOK.EXE"

REM Open TeXstudio
start "" "C:\Users\YourUsername\Desktop\TeXstudio.lnk"

REM Open Chrome
start "" "C:\Path\To\Chrome\Application\chrome.exe"

REM Open PowerShell and navigate to the log file path
start "" powershell.exe -NoExit "cd C:\Path\To\Your\Log\File; Get-Content .\my_package.log -Wait"

REM Open File Explorer at the internship folder
start "" explorer.exe "C:\Path\To\Your\Internship\Folder"

REM Add a pause at the end of the script to keep the command window open
pause
