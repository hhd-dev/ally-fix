# Ally (normal and X) RGB Rainbow fix
This readme is currently on the TODO. It will be updated once the fix has
been verified to work.

Packaging will be worked on for both Windows and Linux.

> [!IMPORTANT]
> The RGB issue was not caused and is in no way associated to Handheld Daemon.
> I choose to provide the fix and packaging for it out of respect to my users.

## Running

To run the fix, head to releases and download either `ally-fix-linux` for
Linux or `ally-fix-windows.exe` for Windows (not yet packaged).

On Linux, right-click the file, go to properties, and allow it to be executed
as a program. Then, right click, open a terminal in the folder, and run
`./ally-fix-linux` to see the output.

On Windows, simply double-click the file to run it. You might have to dismiss
certain Defender warnings or run as administrator. A terminal will open and
inform you if the fix was applied.

This readme will become more in-depth as the fix becomes more tested.

You always have the option of running the fix from source, by cloning this 
repository and running `python3 ally-fix.py` in the terminal.
The only requirement for running it from source is having libusb and Python 3.8+.