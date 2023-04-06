---
layout: post
title: ""
categories: jekyll
permalink : /notes/runfromdebugger
---

# Red Team 101 : Anti debugging

One of the ways to understand how an application works is to perform debugging on it.

This allows us to start and stop the execution of code, step through the code instruction by instruction, inspect the values of registers, memory locations, and others.

During a red team engagement, it is considered a "jackpot" if we manage to create a payload that can evade detections.

![51](/musubi/assets/debugmenot/unatco.gif)

However, sometimes we don't want the techniques that we used in our payload to be discovered by anyone else—maybe it's our secret recipe or the 'ultimate' technique.

One of the checks that we would put in our code is an anti-debugging technique.

This can easily be achieved by using IsDebuggerPresent() function from kernel32.dll.

This is an example proof of concept that uses above function.

## Without debugger

![wod](/musubi/assets/debugmenot/isdbgpresent1.PNG)


## With debugger

![wd](/musubi/assets/debugmenot/isdbgpresent2.PNG)

This is one of the well known anti debugging technique used. So, there are possibilities that this function will be targeted , modified or bypassed.

Another way is by using *AdfCloseHandleOnInvalidAddress* function documented by vxunderground and authored by Checkpoint Research.

[source code](https://github.com/vxunderground/VX-API/blob/main/VX-API/AdfCloseHandleOnInvalidAddress.cpp)

```
#include "Win32Helper.h"

BOOL AdfCloseHandleOnInvalidAddress(VOID)
{
	__try
	{
#pragma warning( push )
#pragma warning( disable : 4312)
		CloseHandle((HANDLE)0xDEADBEEF);
#pragma warning( pop )
		return FALSE;
	}
	__except (EXCEPTION_INVALID_HANDLE == GetExceptionCode() ? EXCEPTION_EXECUTE_HANDLER : EXCEPTION_CONTINUE_SEARCH)
	{
		return TRUE;
	}

	return FALSE;
}
```

Basically, this function triggers an error by closing an invalid handle. Since this exception is likely to be caught by the debugger, the application or binary will detect the debugger's presence and continue its appropriate next course of action.

## Let's try it out.

![adf](/musubi/assets/debugmenot/final.gif)

This may be just a basic technique, and there are ways to bypass this technique.
However, this can be a good start to learn more about anti-debugging!


Ciao!

![c](/musubi/assets/debugmenot/ciao.gif)
