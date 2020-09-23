<div align="center">

## The Missing CloseProcess\(\) API \(Well, Sort Of\)\!


</div>

### Description

Have you ever wanted to CLEANLY shutdown an external application that you only had a ProcessID to? I've always wished that there was the equivalent to the TerminateProcess() API that would send the WM_CLOSE message to the application, allowing it to shutdown cleanly, not abruptly as TerminateProcess() does. Because of this, I began a quest to either find such an API or to build one myself...

Basically, the only way to successfully shutdown an application who's window title you do not know beforehand on Win95/98 is to use the TerminateProcess() API. WinME seems to correctly process the WM_CLOSE message to its top-level windows, so it can be excluded from this even though according to MSDN it functions the same as Win95/98. Now, use of the TerminateProcess() API is frowned upon because according to the MSDN article http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/terminateprocess.asp :

"The TerminateProcess function is used to unconditionally cause a process to exit. Use it only in extreme circumstances. The state of global data maintained by dynamic-link libraries (DLLs) may be compromised if TerminateProcess is used rather than ExitProcess."

But... with the use of the WM_ENDSESSION message this downside seems to be obverted, because to this MSDN article http://msdn.microsoft.com/library/default.asp?url=/library/en-us/sysinfo/base/wm_endsession.asp (which is actually for the WM_QUERYENDSESSION message):

"Windows 95/98/Me: After all applications return TRUE for this message (WM_QUERYENDSESSION), they receive the WM_ENDSESSION and they are terminated."

So if I'm reading that correctly, at shutdown Win95/98 sends the WM_QUERYENDSESSION to all top-level windows, followed by the WM_ENDSESSION then (assuming all applications are happy with the shutdown) they are then terminated assumedly via the TerminateProcess() API! Now this process also occurs at a logoff, so it *should* be reasonable to assume that all of the downsides of the use of the TerminateProcess() API are solved by first sending the window the WM_ENDSESSION and a True in the wParam argument because the application is *supposed* to clean up after itself when it receives the WM_ENDSESSION message.

In my research, I found a number of sites concerning the incorrect handling of the WM_ENDSESSION by Delphi applications. They advised their developers to hook the message and fully close their apps (destroy objects, etc) at that time in order to avoid any errors. So as long as I've not missed any important nuances, I believe that with the help of Ark's comment I've stumbled across the answer to my question!

So in order to put this into a nice little package, I've implemented the fabled CloseProcess() API I used to wish for...

As always… if you find a bug please let me know. Thanks and enjoy!
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |2002-11-16 16:24:28
**By**             |[Nick Campbeln](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/nick-campbeln.md)
**Level**          |Beginner
**User Rating**    |4.9 (83 globes from 17 users)
**Compatibility**  |VB 6\.0
**Category**       |[Windows API Call/ Explanation](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/windows-api-call-explanation__1-39.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[The\_missin14985511162002\.zip](https://github.com/Planet-Source-Code/nick-campbeln-the-missing-closeprocess-api-well-sort-of__1-40781/archive/master.zip)








