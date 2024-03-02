+++
title = "The X Window System"
date = "2024-03-02T17:27:30+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["linux", "x", "gui"]
keywords = ["", ""]
description = "An overview of the X Window System" 
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

The X Window System is a framework for creating and managing graphical user interfaces in Unix-like systems.

## Nomenclature
- The **X Window System** is a network-transparent, client/server system. It is composed of an *X server* and of *X clients*.
- An **X Server** is an application that runs on a system with inputs (e.g. mouse, keyboard) and outputs (e.g. screen). It interfaces with the kernel to process inputs from the input devices, render graphics via video drivers and push bitmaps on the output devices.
- An **X client** is any application which requests rendering of graphics to an X server and receive events from this server (e.g. from input devices). It can run locally or anywhere on the network.
- The **X11** protocol is the network protocol that X servers and X clients use to talk.
- The Xorg server is the most popular implementation of an X server.
- **Xlib** and **xcb** are client libraries that implement X11.

The X Window System having a client/server implementation means that you can have an heavy cpu consuming application (e.g. matlab) acting as an X client on a buffy remote server and display it on a light laptop running an X server. This can be done for example via ssh, using XForwarding. `ssh` into the remote server with the `-X` flag, then run the `matlab` command, and the matlab gui appears on your laptop for you to use with the server's cpu cores.

## Configuration
The Xorg server is configured through the `/etc/X11/xorg.conf` file and the `/etc/X11/xorg.conf.d/` directory.

The `startx` script start up the X server. It's installed by xinit. It runs the X server and execute the `~/.xinitrc` file or read the value of the `$XSESSION` variable.

`Ctrl`+`Alt`+`Backspace` will terminate the session and restart X in case something goes wrong.

To install an appropriate video driver for your hardware, identify your graphic card(s) with:
```bash
lspci -v | grep -A1 -e VGA -e 3D
```
and install the appropriate video driver.

## Tools
- `xrand` is a CLI tool used to manage screen resolutions, rotation and placement. Example:
```bash
# List detected screens
xrandr
# Change resolution
xrandr --output DP-3 --mode 1920x1080
# Change frequency
xrandr --output DP-3 --rate 75
```
An xrandr script/command can be added to the `.xinitrc` file to be executed on every X startup.

- `xtruss` is the equivalent of `strace` for logging interactions between a GUI application and the X Server.
- `xprop` is a tool for displaying window and font properties in an X server, like `WM_NAME` (title of the window) or `_NET_WM_ICON` (its icon).

## Links
- [Xorg](https://wiki.archlinux.org/title/xorg)
- [X11 Forwarding](https://wiki.archlinux.org/title/OpenSSH#X11_forwarding)
- [X Window System](https://en.wikipedia.org/wiki/X_Window_System_protocols_and_architecture)
- [xtruss](https://www.chiark.greenend.org.uk/~sgtatham/xtruss/)
- [xprop](https://www.x.org/releases/X11R7.5/doc/man/man1/xprop.1.html)
- [Display Manager](https://wiki.archlinux.org/title/Display_manager)
