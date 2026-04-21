# Activity-Tracker Version 1.0

**An application for tracking time spent in other applications.**

**The core application is written entirely in C#.**

## Requirements
### Minimum
* [.NET 10.0](https://dotnet.microsoft.com/ru-ru/download/dotnet/10.0) or greater.

* #### Windows:
  * Windows 10
  * Windows 11
 
## How to use:
### Click on the "Create Timer" button and select the application to monitor.
### The timer will run as long as you are in the main tracked application and perform actions (at least once every 5 seconds).

## How it work:

### [Windows Forms Timer](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.timer?view=windowsdesktop-10.0)
System.Windows.Forms.Timer is a component that raises the Tick event on the UI thread at regular intervals. In our application, each timer updates the displayed elapsed time for the specific tracked window.

### [Windows Process List](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.process?view=net-10.0)
The System.Diagnostics.Process class is used to retrieve information about running processes. The [GetProcesses()](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.process.getprocesses?view=net-10.0) method returns an array of all active processes, and the [MainWindowTitle](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.process.mainwindowtitle?view=net-10.0) property contains the caption of the main window of a process. This allows us to build a list of windows available for tracking.

### Determining the Active Window (WinAPI)
To identify which window currently has focus, we call the [GetForegroundWindow](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getforegroundwindow) function from user32.dll. It returns a handle to the window the user is currently working with.
Then, using [GetWindowThreadProcessId](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getwindowthreadprocessid), we obtain the process identifier (PID) that owns that window. Comparing the active window's PID with the stored PID of the tracked application determines whether the required window is active.

### Tracking User Idle Time (WinAPI)
The [GetLastInputInfo](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getlastinputinfo) function from user32.dll retrieves the time of the last input event (keyboard or mouse) system-wide. The [LASTINPUTINFO](https://learn.microsoft.com/en-us/windows/win32/api/winuser/ns-winuser-lastinputinfo) structure contains the dwTime field, which stores the number of milliseconds that have elapsed since the operating system started until the last input.
By comparing this value with the current system tick count ([Environment.TickCount](https://learn.microsoft.com/en-us/dotnet/api/system.environment.tickcount?view=net-10.0)), we calculate the idle time and pause the timer if the user has been inactive for more than 5 seconds.


## About
  * This project is in the development stage. Should you have any suggestions for improvements or encounter any issues, we encourage you to submit them via the [issue](https://github.com/SaveKenny01/Dll-Injector/issues).
  * It might be a heuristic reaction from your antivirus, in which case try disabling it.
  ## DISCLAIMER: I AM NOT RESPONSIBLE FOR ANY ILLEGAL USAGE OF THIS TOOL.
