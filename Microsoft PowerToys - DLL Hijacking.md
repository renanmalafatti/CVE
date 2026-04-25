# Microsoft PowerToys - DLL Hijacking / DLL Injection Vulnerability

## Overview

A DLL Hijacking vulnerability was identified in Microsoft PowerToys due to insecure loading of Dynamic Link Libraries (DLLs) without proper integrity validation or trusted path enforcement.

The application may load attacker-controlled DLL files placed in locations searched during startup, allowing malicious code execution in the context of the PowerToys process.

## Affected Component

DLL loading mechanism during application startup.

## Impact

An attacker with local access may place a malicious DLL in a directory searched by the application, resulting in arbitrary code execution when Microsoft PowerToys is launched.

Depending on the user privileges, this issue may allow:

- Arbitrary command execution  
- Persistence on the system  
- Abuse of user privileges  
- Execution of malicious payloads  

## Vulnerability Details

The issue occurs because the application loads external DLL dependencies without validating:

- Digital signatures  
- File integrity  
- Trusted absolute paths  
- Library authenticity  

This allows a malicious DLL with the same expected name to be loaded instead of the legitimate library.

## Proof of Concept (PoC)

A malicious DLL was created and placed in the application execution directory.

When Microsoft PowerToys started, the malicious DLL was loaded automatically and executed attacker-controlled code.

Example payload:

```cpp
#include <windows.h>
#include <cstdlib>

BOOL WINAPI DllMain(HANDLE hDll, DWORD dwReason, LPVOID lpReserved)
{
    switch (dwReason)
    {
        case DLL_PROCESS_ATTACH:
            system("whoami > C:\\Users\\xxx\\path\\whoami.txt");
            break;

        case DLL_PROCESS_DETACH:
        case DLL_THREAD_ATTACH:
        case DLL_THREAD_DETACH:
            break;
    }

    return TRUE;
}
```
<img width="803" height="398" alt="image" src="https://github.com/user-attachments/assets/9ca25039-32d9-4eac-b2f8-9867acc3cdb9" />

<img width="669" height="125" alt="image" src="https://github.com/user-attachments/assets/cd7ac8b5-cc6b-48af-809f-2827482deb56" />

<img width="393" height="362" alt="image" src="https://github.com/user-attachments/assets/401fb149-305c-4706-9390-378742892b10" />

<img width="615" height="166" alt="image" src="https://github.com/user-attachments/assets/c3722e44-3b36-410d-a87a-11f17cae2ba3" />

