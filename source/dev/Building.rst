Building the MIDIMonster
========================

This document is intended to serve as a documentation for anyone interested in building
the provided source code of the MIDIMonster.

The MIDIMonster build system is very much targeted towards command-line, unix-oid systems,
stemming from the developers preference for working with Linux systems. This has the additional
benefits of being easily automatable, for example to provide automated release and preview binary
builds.

With that said, the build architecture follows the standards set and used by most C projects,
and thus there should be little to no problem building this project with any set of standards-compliant
compiler/linker setup on any of the supported platforms.

General
-------

This guide assumes a basic familiarity with (Debian) Linux. Basic tasks such as installing
a system and opening a terminal are outside of the scope of this document. Knowledge of the
standard Linux user accounts and the file system is required for just about any kind of
software development.

This guide was written for Debian, with the commands given being specific to Debian's `apt-get`
package manager. As Ubuntu is a derivative of Debian, most instructions will probably work there, too.
The code itself is written independently of any OS, so with some dependency-wrangling it is possible
to build the MIDIMonster on just about any platform with basic POSIX-compatibility.

This guide is not maintained in lockstep with the core code. It may, over time, diverge
somewhat from what the actual build steps in the repository are, especially with respect
to the dependency list as new backends are added. This guide will try to note locations where
these diversions may be sooner rather than later.

Prerequisites
-------------

To build the MIDIMonster using the provided tooling, depending on which system you want to build for,
different things are required.

To build the MIDIMonster for Linux (on Linux)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In principle, any Linux distribution will be able to build the MIDIMonster. However the method of
installation as well as the names of software dependencies will vary. In the interest of
keeping this guide short, this guide will focus on Debian 10 (the `stable` release as of the
time of writing).

Acquire an installation of Debian 10, for example by using a virtual machine or installing
it onto a computer. Familiarize yourself with the method of opening a shell, either in a
graphical terminal or using text mode, and accessing the superuser account `root`.

If you are intending to only use this system to build the MIDIMonster, you may choose to use
only the `root` user on the system. Normally, distributions request you create a nonprivileged
user account in addition to the `root` superuser account. You will need to use the `root`
account to perform system-level tasks such as installing software dependencies. You can also
use that account to build the MIDIMonster. However if you intend to use the system for other
things, the build steps can be performed by any nonprivileged user.

As the `root` user, install the tools required for the basic compilation of software written
in C as well as the `git` source control system using the following command:

```
apt-get install build-essential pkg-config git
```

Fetch a copy of the source code repository by cloning it using `git`. Run this command as the
user you want to use for the build process and in a directory belonging to that user (a user's
`home` directory is commonly used for that purpose):

```
git clone https://github.com/cbdevnet/midimonster
```

Install the required software dependencies of the MIDIMonster, again as the `root` user. The
list of dependencies may change in time when new backends are added, requiring new dependencies, or
dependencies may release newer versions. As this guide is not maintained in lockstep with main
repository, check the main `README` file for an up-to-date list.

```
apt-get install libasound2-dev libevdev-dev liblua5.3-dev libola-dev libjack-jackd2-dev libssl-dev python3-dev
```

Some of these dependencies add up to quite some space. For experienced users who may not want to
build all supported backends, it is possible to only install the dependencies required for the
backends that they are intending to build. This will require additional attention during the actual
build steps.

To build the MIDIMonster for Windows (on Linux)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
For reasons of accessibility, automatability, ease-of-use and licensing, building for Windows
employs a cross-compilation process from Linux, directly producing PE-format executables
(`.exe` files for executables, `.dll` files for plugins and libraries).

Follow the prerequisite steps for the Linux (on Linux) build, and additionally install the
package `mingw-w64`, which contains the cross-compiler for Windows using the command

```
apt-get install mingw-w64
```

To build the Lua backend for Windows, a copy of the `lua53.dll` binary is required. For various
reasons, this file can and will not be supplied with the MIDIMonster itself. Acquire a copy of the
file (for example from the [luabinaries project](http://luabinaries.sourceforge.net/download.html)
and place it into the project root directory.

To build the MIDIMonster for OSX (on OSX)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
OSX has it's own way of doing many things, which unfortunately includes such basic tasks as setting
up a workable development environment. Instructions on how to set up the `brew` package manager may
be found on that [project's homepage](https://brew.sh/).

Once that has been done, install the software dependencies for the MIDIMonster using

```
brew install ola lua openssl jack python3
```

If your system still has an installation of a Python 2.7 release, you may need to overwrite that
using the command

```
brew link --overwrite python
```

Using the makefile
------------------

The `makefile` is an instruction file for the `make` utility. It contains the steps required
to build the final binaries for the MIDIMonster and, implicitly, the order in which to take them.

Building for Linux
------------------

Building for Linux
------------------

Building for Windows
--------------------

Building for OSX
----------------

export CFLAGS="$CFLAGS -I/usr/local/opt/openssl@1.1/include"
export LDFLAGS="$LDFLAGS -L/usr/local/opt/openssl@1.1/lib"

Building manually
-----------------

Creating release tarballs
-------------------------

Building Debian Packages
------------------------

Discussion of the makefile
--------------------------
