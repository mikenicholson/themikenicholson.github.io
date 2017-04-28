---
layout: post
title: "Debugging X11"
---

For the last few weeks I've been working to port one of our products from Qt 4.8 to Qt 5.8.  Most
of the process has been relatively painless with the exception of our system tray.  The system tray
relise on some X11 wizardry to embed windows from one application into another (see the
[System Tray Protocol Specification](https://specifications.freedesktop.org/systemtray-spec/systemtray-spec-0.2.html)
 and XEmbed)

Qt 4 support for this wizardy was excellent but I suspect it is not widely used and is broken in
Qt5. In the course of debugging these issues I have come across a few tools that are
indespensible for troubleshooting X1 problems.

# Xephyr - An X server inside your X server

Some applications, like my misbehaving system tray, expect to be the only application of their type
being run on a given screen or display. The system tray expects to be the only Selection Owner for a
given screen. If you're running a deaktop like KDE or Gnome on your development system and trying to
run the application you're debugging you can end up with your application conflicting with the
desktop environment. 

Xephyr prevents this by creating a new X server that outputs to a window in you current session - an
X server within an X server.  This lets any applications behave as they would on your target system.

Xephyr is available through your package manager. On Ubuntu it can be installed with

TODO finish this

```bash 
$ sudo apt-get install xserver-xephyr
```

To start a new display with Xephyr 

```bash
$ # TODO 
```

And to run an application under your Xephyr display just set the `DISPLAY` environment variable to
match the display number you specified when starting Xephyr.

```bash
DISPLAY=:3 xeyes
```

If everything worked correctly you should have a new window with `xeyes` running inside it.

TODO: Picture of xeyes running

## xtrace - The Ultimate X11 Interceptor

Some days things just work. Other days nothing works.  On those days you find yourself pouring
through Qt's xcb backend trying to figure out why your applications event handler isn't recieving
specific X events.  If you're doing this than chances are you need xtrace. 

xtrace acts as a man-in-the-middle for between your applicaiton and the X server your application is
drawing to.  It intercepts and pretty prints every communication beteween your application the X
server your application is drawing to.  This lets you spy on every interaction between the X serfver
and your app.

Again, install xtrace thorugh your favorite package manager. On Ubuntu 

```bash
$ sudo apt-get install xtrace
```

Running your application under xtrace is easy 

```bash
$ xtrace xeyes
```


## xlsatoms

TODO: Explain atoms in X 
