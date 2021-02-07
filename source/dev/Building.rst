Building the MIDIMonster
========================

This document is intended to serve as point of reference for anyone interested in building
the provided source code of the MIDIMonster. It is not yet finished.

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
these diversions may be sooner rather than later. To see what steps the automated build system and
the CI system is using, experienced users may wish to look at the `Makefiles` in both the project root
as well as the `backends` subdirectory, as well as the `.ci.sh`  and `.travis.yml` files.

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
in C as well as the `git` source control system using the following command::

	apt-get install build-essential pkg-config git

Fetch a copy of the source code repository by cloning it using `git`. Run this command as the
user you want to use for the build process and in a directory belonging to that user (a user's
`home` directory is commonly used for that purpose)::

	git clone https://github.com/cbdevnet/midimonster

Install the required software dependencies of the MIDIMonster, again as the `root` user. The
list of dependencies may change in time when new backends are added, requiring new dependencies, or
dependencies may release newer versions. As this guide is not maintained in lockstep with main
repository, check the main `README` file for an up-to-date list::

	apt-get install libasound2-dev libevdev-dev liblua5.3-dev libola-dev libjack-jackd2-dev libssl-dev python3-dev

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
package `mingw-w64`, which contains the cross-compiler for Windows using the command::

	apt-get install mingw-w64

To build the Lua backend for Windows, a copy of the `lua53.dll` binary is required. For various
reasons, this file can and will not be supplied with the MIDIMonster itself. Acquire a copy of the
file (for example from the `luabinaries project <http://luabinaries.sourceforge.net/download.html>`_)
and place it into the project root directory.

To build the MIDIMonster for OSX (on OSX)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
OSX has it's own way of doing many things, which unfortunately includes such basic tasks as setting
up a workable development environment. Instructions on how to set up the `brew` package manager may
be found on that `project's homepage <https://brew.sh/>`_.

Once that has been done, install the software dependencies for the MIDIMonster using::

	brew install ola lua openssl jack python3

If your system still has an installation of a Python 2.7 release, you may need to overwrite that
using the command::

	brew link --overwrite python

Building using the makefile
---------------------------

The `makefile` is an instruction file for the `make` utility. It contains the steps required
to build the final binaries for the MIDIMonster and, implicitly, the order in which to take them.
This guide will take a closer look at the architecture of the `makefile` in a later section.

Building for Linux
^^^^^^^^^^^^^^^^^^
To run the build steps for a Linux version, just run `make`. This will implicitly call make with
the `all` target, building the core as well as all backends that are configured for Linux compatibility.

`make` will try to perform only necessary actions, skipping rebuilds of already built files where the
source files have not been changed. To force a complete rebuild, the invocation::

	make clean all

may be used to perform a `clean` before building, thus forcing all binaries to be rebuilt.
The `clean` target can also be used on it's own to clean up any binary files left from a build process.

To build specific binaries, for example a single object file, `make` can be invoked like this::

	make midimonster

which will then only build the core binary, not the backends. In a similar fashion, only specific
backends can be built within the `backends/` directory.

The build process specified within the `makefile` takes a number of parameters using environment
variables, among others the standard `CC`, `LDLIBS`, `CFLAGS` and `LDFLAGS` parameters. These can be
used by experienced users as well as automated processes to influence the build process.
Some of these variables are discussed in a later section of this document.

The `makefile` provides additional targets, some of which are discussed in a later section.

Building for Windows
^^^^^^^^^^^^^^^^^^^^
The `makefile` provides a target named `windows`, which overwrites some of the variables for the build
process with values that result in a cross-compiler being used, as well as performing some Windows-specific
steps. When executing::

	make windows

the build process will compile the code to a set of Windows-specific files, including `midimonster.exe` and
the backend shared libraries as DLL files. These can then be copied using either the deploy steps described
later in this document, or run using an emulator.

Building for OSX
^^^^^^^^^^^^^^^^
The OSX build is conceptually very similar to the Linux build, in that it uses the same tooling, albeit
with a different default compiler as OSX uses `clang` by default. Additionally, the `openssl` library, which
is used for the `maweb` backend, has some issues on OSX, which require the following commands to be run
before building as a workaround::

	export CFLAGS="$CFLAGS -I/usr/local/opt/openssl@1.1/include"
	export LDFLAGS="$LDFLAGS -L/usr/local/opt/openssl@1.1/lib"

This sets up some paths that are (to the knowledge of the author) not easily accessible via established
protocols. Should you have further information on how to get this information programmatically, please
contact the authors. After performing these workarounds, use the `make` command in the same terminal to
build the MIDIMonster OSX binaries.

Building manually
-----------------
This section will describe the basic build steps which are encoded in the `makefile`. It will focus on the
Linux build for this purpose. Other systems follow similar protocols. If your main interest is in experimenting
with the source code, this section will not be of interest. If you are interested in integrating new build
systems or porting the build to another system, this section may hold value.

Building the core
^^^^^^^^^^^^^^^^^
The core consists of a set of object files. These can be found in the `makefile` as the assignment to the
`OBJS` variable. At the time of this writing, the object files are `config.o`, `backend.o` and `plugin.o`.

Each of these object files is built from a corresponding C source file. Additionally, some of these depend
on other source files within the core tree. The `makefile` supplies some additional arguments to hide non-API
symbols from the export table for the compilation unit.

A minimal compilation command for a single unit would look like this::

	cc -c -o config.o config.c

Once all the object files are built, they can be passed to the compilation of the core binary::

	cc -Wl,-export-dynamic midimonster.c config.o backend.o plugin.o -ldl -o midimonster

The core executable requires linking against `libdl` (using the `-ldl` linker flag), which provides the functionality
to load plugins (the backends) at runtime. The `-Wl,-export-dynamic` linker flag adds the plugin-accessible API to the
dynamic symbol table, so it can be used from runtime-loaded plugins.

Building a backend
^^^^^^^^^^^^^^^^^^
All backends consist of C header file, a C source file, and a markdown document containing the backend
documentation. Backends are shared objects (`.so` ELF files on Linux).

A minimal invocation to build a single backend would be::

	cc -fPIC -I../ backend.c -o backend.so -shared

The `-fPIC` and `-shared` flags tell the compiler and linker to create runtime-loadable shared libraries.
The additional include path (`-I../`) puts the `midimonster.h` API header file into the include search path
for the backends.

Most backends will require linking against their specific libraries (for example, `libasound`/`-lasound` for the
`midi` backend).

The network-based backends share a lot of overlapping code via a MIDIMonster-internal library
called `libmmbackend`. This library can be built using the invocation::

	cc -fPIC -I../ -c -o libmmbackend.o libmmbackend.c

Creating release tarballs
-------------------------

Building Debian Packages
------------------------

Discussion of the makefile
--------------------------
