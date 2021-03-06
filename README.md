pyOCD
=====

pyOCD is an open source Python package for programming and debugging Arm Cortex-M microcontrollers
using multiple supported types of USB debug probes. It is fully cross-platform, with support for
Linux, macOS, and Windows.

Several command line tools are provided that cover most use cases, or you can make use of the Python
API to enable low-level target control. A common use for the Python API is to run and control CI
tests.

Three tools give you total control over your device:

- `pyocd-gdbserver`: GDB remote server allows you to debug using gdb via either
    [GNU MCU Eclipse plug-in](https://gnu-mcu-eclipse.github.io/) or the console.
- `pyocd-flashtool`: Program and erase an MCU's flash memory.
- `pyocd-tool`: Interactive REPL control and inspection of the MCU.

The API and tools provide these features:

-  halt, step, resume control
-  read/write memory
-  read/write core registers
-  set/remove hardware and software breakpoints
-  set/remove watchpoints
-  write to flash memory
-  load binary, hex, or ELF files into flash
-  reset control
-  access CoreSight DP and APs
-  and more!


Requirements
------------

- Python 2.7.9 or later, or Python 3.6.0 or later
- macOS, Linux, or Windows 7 or newer
- Microcontroller with an Arm Cortex-M CPU
- Supported debug probe
  - [CMSIS-DAP](http://www.keil.com/pack/doc/CMSIS/DAP/html/index.html), such as an on-board debug
    probe using [DAPLink](https://os.mbed.com/handbook/DAPLink) firmware.
  - STLinkV2, either on-board or the standalone version.


Status
------

PyOCD is functionally reliable and fully useable.

The API is considered unstable because we are planning some breaking changes to bring the naming
convention into compliance with PEP8 prior to releasing version 1.0. We also plan to merge the three
command line tools into a single tool.


Documentation
-------------

The pyOCD documentation is located [in the docs directory](docs/).


Installing
----------

The latest stable version of pyOCD may be installed via [pip](https://pip.pypa.io/en/stable/index.html)
as follows:

```
$ pip install -U pyocd
```

The latest pyOCD package is available [on PyPI](https://pypi.python.org/pypi/pyOCD/) as well as
[on GitHub](https://github.com/mbedmicro/pyOCD/releases).

To install the latest prerelease version from the HEAD of the master branch, you can do
the following:

```
$ pip install --pre -U https://github.com/mbedmicro/pyOCD/archive/master.zip
```

You can also install directly from the source by cloning the git repository and running:

```
$ python setup.py install
```

Note that, depending on your operating system, you may run into permissions issues running these commands.
You have a few options here:

1. Under Linux, run with `sudo -H` to install pyOCD and dependencies globally. (Installing with sudo
   should never be required for macOS.)
2. Specify the `--user` option to install local to your user.
3. Run the command in a [virtualenv](https://virtualenv.pypa.io/en/latest/)
   local to a specific project working set.

### libusb installation

[pyusb](https://github.com/pyusb/pyusb) and its backend library [libusb](https://libusb.info/) are
dependencies on all supported operating systems. pyusb is a regular Python package and will be
installed along with pyOCD. However, libusb is binary shared library that does not get installed
automatically via pip dependency management.

How to install libusb depends on your OS:

- macOS: use Homebrew: `brew install libusb`
- Linux: should already be installed.
- Windows: download libusb from [libusb.info](https://libusb.info/) and place the DLL in your Python
  installation folder next to python.exe.

### udev rules on Linux

If you encounter an issue on Linux where `pyocd-tool list` won't detect attached boards without
sudo, the reason is most likely USB device access permissions. In Ubuntu 16.04+ these are handled
with udev and can be solved by adding a new udev rule.

An example udev rule file is included in the [udev](https://github.com/mbedmicro/pyOCD/tree/master/udev)
folder in the pyOCD repository. Just copy this file into `etc/udev/rules.d` to enable user access
to both [DAPLink](https://os.mbed.com/handbook/DAPLink)-based debug probes as well as STLinkV2 and
STLinkV3.

If you use different, but compatible, debug probe, you can check the IDs with ``dmesg`` command.

   -  Run ``dmesg``
   -  Plug in your board
   -  Run ``dmesg`` again and check what was added
   -  Look for line similar to ``usb 2-2.1: New USB device found, idVendor=0d28, idProduct=0204``


Standalone GDB server
---------------------

When you install pyOCD via pip or setup.py, you will be able to execute the following in order to
start a GDB server powered by pyOCD:

```
$ pyocd-gdbserver
```

You can get additional help by running ``pyocd-gdbserver --help``.

Example command line GDB session showing how to connect to a running `pyocd-gdbserver` and load
firmware:

```
$ arm-none-eabi-gdb application.elf

<gdb> target remote localhost:3333
<gdb> load
<gdb> monitor reset
```

The `pyocd-gdbserver` executable is also usable as a drop in place replacement for OpenOCD in
existing setups. The primary difference is the set of gdb monitor commands.


Recommended GDB and IDE setup
-----------------------------

The GDB server works well with [Eclipse](https://www.eclipse.org/) and the [GNU MCU Eclipse
plug-ins](https://gnu-mcu-eclipse.github.io/). GNU MCU Eclipse fully supports pyOCD with an included
pyOCD debugging plugin.

To view peripheral register values either the built-in GNU MCU Eclipse register view can be used, or
the Embedded System Register Viewer plugin can be installed. These can be installed from inside
Eclipse using the following software update server addresses:

- GNU MCU Eclipse: http://gnu-mcu-eclipse.sourceforge.net/updates
- Embedded System Register Viewer: http://embsysregview.sourceforge.net/update

In Eclipse, select the "Help -> Install New Software…" menu item. Then either click the "Add…"
button and fill in the name and URL from above (once for each site), or simply copy the URL into the
field where it says "type or select a site". Then you can select the software to install and click
Next to start the process.


Development setup
-----------------

Please see the [Developers' Guide](docs/DEVELOPERS_GUIDE.md) for instructions on how to set up a
development environment for pyOCD.


Contributions
-------------

We welcome contributions to pyOCD in any area. Please see the [contribution
guidelines](CONTRIBUTING.md) for details.

To report bugs, please [create an issue](https://github.com/mbedmicro/pyOCD/issues/new) in the
GitHub project.


License
-------

PyOCD is licensed with Apache 2.0. See the [LICENSE](LICENSE) file for the full text of the license.

Copyright © 2006-2018 Arm Ltd
