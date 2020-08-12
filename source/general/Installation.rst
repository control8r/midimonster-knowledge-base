.. highlight:: console

############
Installation
############

The easiest way to get started is to `download a binary build <https://github.com/cbdevnet/midimonster/releases>`_ and use the pre-built binaries.
However, you can always build your own version using either the last tagged release or the current
development status.


===================
Using the Installer
===================

An easy way to install either a release or a development version MIDIMonster and its dependencies on a Linux system
is to use the provided `installer script <https://github.com/cbdevnet/midimonster/blob/master/installer.sh>`_.

To use it, just run the following commands on your machine as the root user:
::

 wget https://raw.githubusercontent.com/cbdevnet/midimonster/master/installer.sh ./
 chmod +x ./installer.sh
 ./installer.sh


The installer can also be used for automating installations or upgrades by specifying additional command line arguments. To see a list of valid arguments, run the installer with the ``--help`` argument.

====================
Building from source
====================

If you want to build **MIDIMonster** of your own, look at our ``README`` in the `MIDIMonster repository <https://github.com/cbdevnet/midimonster#building-from-source>`_.
