# CLion unofficial Flatpak wrapper
This version is running inside a container and is therefore not able
to access toolchains on your host system!

## How to run commands on the host system
To execute commands on the host system, run inside the sandbox:

  $ flatpak-spawn --host <COMMAND>

To make the Integrated Terminal automatically use the host system's shell,
you can modify `Settings > Tools > Terminal > Shell path` to

  /usr/bin/env -- flatpak-spawn --host bash

## Flatpak standard SDK
This Flatpak provides a standard development environment (gcc, gdb, make,
cmake, python, etc.). To see what's available:
```
$ flatpak run --command=sh com.jetbrains.CLion
$ ls /usr/bin (shared runtime)
$ ls /app/bin (bundled with this flatpak)
```

## Flatpak SDK extensions
To get support for additional languages, you have to install SDK extensions, e.g.
```
$ flatpak install flathub org.freedesktop.Sdk.Extension.golang
```

To enable selected extensions, set FLATPAK_ENABLE_SDK_EXT environment variable
to a comma-separated list of extension names (name is ID portion after the last dot):

`$ FLATPAK_ENABLE_SDK_EXT=golang,xxx flatpak run com.jetbrains.CLion`

To make this persistent, set the variable via Flatpak override:

`$ flatpak override --user com.jetbrains.CLion --env=FLATPAK_ENABLE_SDK_EXT="golang,xxx"`

You can use to find others

`$ flatpak search <TEXT>`

## Embedded development
Serial ports and USB devices (ST-Link, J-Link, CMSIS-DAP, OpenOCD, etc.)
are accessible from the sandbox. The udev rules of your host system apply:
your user needs the usual permissions (e.g. the `dialout` group for serial
ports).

## Docker and Podman
The Docker and Podman sockets are exposed to the sandbox, so container
toolchains can be used.
