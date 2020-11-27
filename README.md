# Component Services Volatile Environment LPE

| Exploit Information |                                   |
|:------------------- |:--------------------------------- |
| Date                | 28.06.2017                        |
| Patched             | Windows 10 RS3 (16299)            |
| Tested on           | Windows 7-10, x86 and x64         |

## Description

In dcomcnfg.exe (Component Services), there is a DLL loading vulnerability that can be exploited by injecting an environment variable.

Let's look at dcomcnfg's manifest:

```xml
<description>Small utility exe to make it easy
to start "mmc.exe %windir%\system32\comexp.msc"</description>
```

This also makes it easy to exploit, so thank you for this small utility. This "small and easy" binary executes mmc.exe with high IL and a load attempt to %SYSTEMROOT%\System32\mmcndmgr.dll will be performed from this auto-elevated process.

Redirecting %SYSTEMROOT% can be achieved through Volatile Environment. For this, we set HKEY_CURRENT_USER\Volatile Environment\SYSTEMROOT to a new directory, which we then populate with our hijacked payload DLL, along with *.clb files from C:\Windows\Registration as they are loaded from our new directory as well.

Then, as we execute dcomcnfg.exe, it will load our payload DLL and also the COM+ components. We need to copy those, too, because the process will otherwise crash.

Our DLL is now executed with high IL. In this example, Payload.exe will be started, which is an exemplary payload file displaying a MessageBox.

## Expected Result

When everything worked correctly, Payload.exe should be executed, displaying basic information including integrity level.

![](https://bytecode77.com/images/pages/component-services-privilege-escalation/result.png)

## Downloads

Compiled binaries with example payload:

[![](http://bytecode77.com/public/fileicons/zip.png) ComponentServicesVolatileEnvironmentLPE.zip](https://bytecode77.com/downloads/ComponentServicesVolatileEnvironmentLPE.zip)
(**ZIP Password:** bytecode77)

## Project Page

[![](https://bytecode77.com/public/favicon16.png) bytecode77.com/component-services-privilege-escalation](https://bytecode77.com/component-services-privilege-escalation)