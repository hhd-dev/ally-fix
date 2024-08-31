# Ally (normal and X) RGB Rainbow fix
This repository contains a fix for the RGB rainbow issue on the Ally and Ally X.
If this issue occurs on your device, Spiral mode in Handheld Daemon and Rainbow
mode in Armoury Crate (when using non-aura) will not work.

When you switch to this mode, either one of the stick leds will be lit in a
static color or neither of the stick leds will be lit.

> [!IMPORTANT]
> The RGB issue was not caused and is in no way associated to Handheld Daemon.
> Read the explanation below for more information.

## Running
To run, head to releases and either download the `ally-fix-linux` executable
for Linux or the `ally-fix-windows.exe` executable for Windows.
Then, double click to run.
The fix will be applied and your Ally RGB will switch into rainbow mode.

On Windows, a console window will open showing the status of the fix.

On Linux, a window will not open.
If the fix does not apply, right-click the file, go to properties, and allow it
to be executed as a program. 
Then, right click, open a terminal in the folder, and run
`./ally-fix-linux` to see the output.

The prepackaged fixes might not load properly on all systems. If you find
an issue with your system, please open an issue.

### Running from Source
You always have the option of running the fix from source, by cloning this 
repository or just downloading `ally-fix.py` and running `python3 ally-fix.py` 
in the terminal.
The only requirement for running it from source is having libusb and Python 3.8+.

## Why Rainbow RGB broke
As Handheld Daemon was being developed, it did not have a strong set of TDP
features and access to fan curves, which made it limitting.
To supplement this functionality, the asusctl/rog center/asus-linux.org suite
of software was included in Bazzite, as part of the `bazzite-ally` image.
The same software suite was also included in other distributions such as Nobara,
CachyOS, and as of version 46 ChimeraOS.

The asus-linux.org family of software is a community run project that aims to
add Linux support to Asus consumer laptops.
While the use of Asus' trademarks in the name of the programs might suggest
otherwise, it is in no way endorsed or developed by Asus, and, apart from some 
limited communication with Asus engineers, is not affiliated with Asus in any way.

The Rainbow RGB issue was caused by all asusctl versions from `6.0.0` to `6.0.11`, with 
version `6.0.12` being the first to stop causing the issue (not fixing it).
In addition, using a `5.0.X` version of the software is known to fix the issue.
Therefore, the question is raised: why did the issue occur in the first place?

One of the commands ran by asusctl to control when the leds were on
**in laptops** (`0x5D, 0xBD, 0x01, X, X, X, X`)
was run on the Ally instead of the proper command.
This unsyncs the zones of the Ally, causing single zone RGB modes to break.
The single-zone functionality is only used by a single RGB mode in Armoury Crate:
Rainbow mode when in the non-aura mode.
This leads to that mode being broken.
The fix in this repository runs the command `0x5D, 0xBD, 0x01, 0xFF, 0xFF, 0xFF, 0xFF`
which syncs the zones and restores the RGB rainbow mode.
As `asusctl` asserts itself and runs on boot, even if the user did not choose
to use it, this means that any user that had `asusctl` installed is affected
by the issue.

In Bazzite, we deprecated the `ally` image and encouraged our users to migrate
in June.
This means that any users that installed Bazzite afterwards, including **all** 
Ally X owners have working RGB rainbow mode.
This fix is for users who used the `bazzite-ally` image before the fixes and
users for other distributions.
This includes Ally and **Ally X** users who installed ChimeraOS 46.2.

We want our users to have a great experience with their devices, and not feel
like they are risking them by using our software.
For this reason, even though we did not cause this issue, we dedicated time in 
developing and testing a fix that can be run both on Windows and Linux, and 
does not require the use of any other software.
Hopefully, this step helps restore some trust that was lost due to this issue.

We will also reach out to Asus so that that command can be nulled on the Ally
and Ally X firmware as part of an MCU update, so that it does not cause any 
issues in the future.

Moving forward, we also plan to start working closer with manufacturers to ensure
that both Handheld Daemon and the kernel shipped by Bazzite are validated to work
on the devices we claim to support.
Handheld Daemon itself already has a multitude of safety checks to ensure its
correct operation, those include: command debouncing, running only commands that
are run by manufacturer software, as they are run by that software, and multiple
DMI and HID checks to ensure only the correct device receives those commands.
Moreover, this issue has caused us to take a closer look at the kernel we ship
and moving forward we will only cherry-pick commits for devices that we have
validated to work or even go a step further and remove commits that are included in
the upstream Linux Foundation kernel.